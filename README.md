# docker-essential
This is a simple learning docker essentials for revising docker fundamentals and commands.

## 1. Create a file named Dockerfile and add the following content:

A Dockerfile lists the instructions needed to build a Docker image. Let's go through the Dockerfile line by line.

### FROM python:3.6.1-alpine

    This is the starting point for your Dockerfile. Every Dockerfile typically starts with a FROM line that is the starting image to build your layers on top of. In this case, you are selecting the python:3.6.1-alpine base layer because it already has the version of Python and pip that you need to run your application. The alpine version means that it uses the alpine distribution, which is significantly smaller than an alternative flavor of Linux. A smaller image means it will download (deploy) much faster, and it is also more secure because it has a smaller attack surface.

    Here you are using the 3.6.1-alpine tag for the Python image. Look at the available tags for the official Python image on the Docker Hub. It is best practice to use a specific tag when inheriting a parent image so that changes to the parent dependency are controlled. If no tag is specified, the latest tag takes effect, which acts as a dynamic pointer that points to the latest version of an image.

    For security reasons, you must understand the layers that you build your docker image on top of. For that reason, it is highly recommended to only use official images found in the Docker Hub, or noncommunity images found in the Docker Store. These images are vetted to meet certain security requirements, and also have very good documentation for users to follow. You can find more information about this Python base image and other images that you can use on the Docker store.

    For a more complex application, you might need to use a FROM image that is higher up the chain. For example, the parent Dockerfile for your Python application starts with FROM alpine, then specifies a series of CMD and RUN commands for the image. If you needed more control, you could start with FROM alpine (or a different distribution) and run those steps yourself. However, to start, it's recommended that you use an official image that closely matches your needs.
   
   
 ### RUN pip install flask

    The RUN command executes commands needed to set up your image for your application, such as installing packages, editing files, or changing file permissions. In this case, you are installing Flask. The RUN commands are executed at build time and are added to the layers of your image.
    
### CMD ["python","app.py"]

    CMD is the command that is executed when you start a container. Here, you are using CMD to run your Python applcation.

    There can be only one CMD per Dockerfile. If you specify more than one CMD, then the last CMD will take effect. The parent python:3.6.1-alpine also specifies a CMD (CMD python2). You can look at the Dockerfile for the official python:alpine image.

    You can use the official Python image directly to run Python scripts without installing Python on your host. However, in this case, you are creating a custom image to include your source so that you can build an image with your application and ship it to other environments.
    
 
### COPY app.py /app.py

    This line copies the app.py file in the local directory (where you will run docker image build) into a new layer of the image. This instruction is the last line in the Dockerfile. Layers that change frequently, such as copying source code into the image, should be placed near the bottom of the file to take full advantage of the Docker layer cache. This allows you to avoid rebuilding layers that could otherwise be cached. For instance, if there was a change in the FROM instruction, it will invalidate the cache for all subsequent layers of this image. You'll see this little later in this lab.

    It seems counter-intuitive to put this line after the CMD ["python","app.py"] line. Remember, the CMD line is executed only when the container is started, so you won't get a file not found error here.

    And there you have it: a very simple Dockerfile. See the full list of commands that you can put into a Dockerfile. Now that you've defined the Dockerfile, you'll use it to build your custom docker image.

## 2. Build the Docker image. Pass in the -t parameter to name your image `python-hello-world`.

```python
$ docker image build -t python-hello-world .
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM python:3.6.1-alpine
3.6.1-alpine: Pulling from library/python
acb474fa8956: Pull complete 
967ab02d1ea4: Pull complete 
640064d26350: Pull complete 
db0225fcac8f: Pull complete 
5432cc692c60: Pull complete 
Digest: sha256:768360b3fad01adffcf5ad9eccb4aa3ccc83bb0ed341bbdc45951e89335082ce
Status: Downloaded newer image for python:3.6.1-alpine
 ---> c86415c03c37
Step 2/4 : RUN pip install flask
 ---> Running in cac3222673a3
Collecting flask
  Downloading Flask-0.12.2-py2.py3-none-any.whl (83kB)
Collecting itsdangerous>=0.21 (from flask)
  Downloading itsdangerous-0.24.tar.gz (46kB)
Collecting click>=2.0 (from flask)
  Downloading click-6.7-py2.py3-none-any.whl (71kB)
Collecting Werkzeug>=0.7 (from flask)
  Downloading Werkzeug-0.12.2-py2.py3-none-any.whl (312kB)
Collecting Jinja2>=2.4 (from flask)
  Downloading Jinja2-2.9.6-py2.py3-none-any.whl (340kB)
Collecting MarkupSafe>=0.23 (from Jinja2>=2.4->flask)
  Downloading MarkupSafe-1.0.tar.gz
Building wheels for collected packages: itsdangerous, MarkupSafe
  Running setup.py bdist_wheel for itsdangerous: started
  Running setup.py bdist_wheel for itsdangerous: finished with status 'done'
  Stored in directory: /root/.cache/pip/wheels/fc/a8/66/24d655233c757e178d45dea2de22a04c6d92766abfb741129a
  Running setup.py bdist_wheel for MarkupSafe: started
  Running setup.py bdist_wheel for MarkupSafe: finished with status 'done'
  Stored in directory: /root/.cache/pip/wheels/88/a7/30/e39a54a87bcbe25308fa3ca64e8ddc75d9b3e5afa21ee32d57
Successfully built itsdangerous MarkupSafe
Installing collected packages: itsdangerous, click, Werkzeug, MarkupSafe, Jinja2, flask
Successfully installed Jinja2-2.9.6 MarkupSafe-1.0 Werkzeug-0.12.2 click-6.7 flask-0.12.2 itsdangerous-0.24
 ---> ce41f2517c16
Removing intermediate container cac3222673a3
Step 3/4 : CMD python app.py
 ---> Running in 2197e5263eff
 ---> 0ab91286958b
Removing intermediate container 2197e5263eff
Step 4/4 : COPY app.py /app.py
 ---> f1b2781b3111
Removing intermediate container b92b506ee093
Successfully built f1b2781b3111
Successfully tagged python-hello-world:latest
```

###3. Verify that your image shows in your image list:

```shell
$ docker image ls

REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
python-hello-world   latest              f1b2781b3111        26 seconds ago      99.3MB
python               3.6.1-alpine        c86415c03c37        8 days ago          88.7MB
```


![My github stats](https://github-readme-stats.vercel.app/api?username=saurabhshcs&show_icons=true)


![Profile views](https://komarev.com/ghpvc/?username=saurabhshcs)

- [x] testing

Follow me on - [Medium](https://saurabhshcs.medium.com) | [Linkedin](https://www.linkedin.com/in/saurabhshcs/) | [YouTube](https://www.youtube.com/channel/UCSQqjPw7_tfx1Ie4yYHbcxQ?pbjreload=102) | [StackOverFlow](https://stackoverflow.com/users/10719720/saurabhshcs?tab=profile)




