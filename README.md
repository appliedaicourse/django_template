# django_template
A simple django template with django, djangorestframework, with docker and travid CI for continuous integration

## Installation steps
#### Creating Dockerfile
- Create a github repository with `python` as a `git-ignore` file and licence as `MIT Licence` with `Readme.md`. You can choose any licence available. Now, clone the repository to your local machine and change the current repository to the github repository with `cd dir_name` 

    ```console    
    foo@bar:~$ cd project_root_dir 
    foo@bar:~/project_root_dir$ touch Dockerfile
    ```

- Make sure you have `docker_CE` and `docker-compose` installed before going further.  Open Docker file and the copy the contents of Dockerfile from this repository and paste them there. All the code in that docker file is documented and we tried our best to explain what each line does. Keep in mind that you need to change the `project_name_in_docker` and `project_name_in_local` to the names that suits your project. This will be maintained throughout the project. Do change it before building your 

#### Create requirements.txt
- Create requirements.txt file and add the contents from this repository. And make sure you create this file in the root folder where the Dockerfile is 
    ```console
    foo@bar:~/project_root_dir$ touch requirements.txt # in the root folder
    ```
- And copy the contents of the requirements.txt in this repository to the file that is created. You might want to change the versions of the project . Here we have Django and djangorestframework with some versions. Add some packages if you need to.

#### Build our Docker Image
- First creat a folder `project_name_in_local`  in the root directory which we specified in the dockerfile. Change the name if you have chagned the Dockerfile
    ```console 
    foo@bar:~/project_root_dir$ mkdir project_name_in_local
    ```
- Build the docker image. It will run the commands that are specified in `Dockerfile` one by one. Mnake sure you are in the project folder( github folder ) which has the Dockerfile, Readme.md etc.,. ( dot included at the end). If everything is done properly, you can see that the docker is running all the steps mentioned on Dockerfile step by step.
    ```console
    foo@bar:~/project_root_dir$ docker build .
    ```
#### Configure docker-compose
- docker-compose is a tool that allows us to run our docker image easily from our project location. It allows us to manage different services that makeup our project very easily. For example, one service might be the python application that we run, and the other service might be the database. For now we only add our python server( django ). You can add different services later ( if needed by your application ).

- Create a `docker-compose.yml` in the root directory and copy the contents from our repository to your file. One main take away is, we add a service that runs the __Django development server__.  As mentioend previously, you can add different servies to this later if you want to.
    
    ```console
    foo@bar:~/project_root_dir$ touch docker-compose.
    ```
    While copying, you might want to replace the folder names ie., `project_name_in_local` and `project_name_in_docker` under the volumes section in this file. After that, let's build our docker image with the docker-compose configuration. ( No dots at the end of the command)
    ```console
    foo@bar:~/project_root_dir$ docker-compose build
    ```
    If everything runs perfectly, we can see ___successfully built 7c3sdsw422___ and ___successfully tagged project_root_dir:latest___ at the end in the output. Now let's create a Django project using the docker configuration that we've created in previous steps.
#### Create a Django project
- We use `docker-compose` to run a command on our image that contains the django dependency. And that will create the project files that we need for our application. We create a project in current directory of the docker( ie., `project_name_in_docker` directory). As we configured the docker in such a way that whatever files that are created in `project_name_in_docker` will be reflected to `project_name_in_local` and vice versa. So the new project will be appeared in `project_name_in_local` as well. 

    ```console
    foo@bar:~/project_root_dir$ docker-compose run app sh -c "django-admin.py startproject project_name ."
    ```
    We have given a different name for this django project. In this we have named it `project_name`. After creating the project, your `project_root_dir` shoould have the following structure.
    ```
    project_root_dir
    └───project_name_in_local
    │   └───project_name
    │       │   __init__.py
    │       │   settings.py
    │       │   urls.py
    │       │   wsgi.py
    │       manage.py
    │   .gitignore
    │   docker-compose.yml
    |   Dockerfile
    |   LICENSE
    |   README.MD
    |   requirements.txt
    ```
- Now I think it is a good time to commit the things and push it to github.
    ```console
    foo@bar:~/project_root_dir$ git add .
    foo@bar:~/project_root_dir$ git commit -m "Setup docker and django project."
    foo@bar:~/project_root_dir$ git push origin master
    ```
#### Enable Travis CI for our Github project
- Travis is a really useful tool for continuous Integration tool that let's us automate some of the test checks on our project everytime we push it to github. We can make it run some things like Python unit tests and python linting everytime we push it to github. It will send us the mail if the build is successful or not. 
    
    For this, sign in with github to travis-ci, and enable our github project to Travis CI.  Now let's configure the travis CI. After we add some configuration file and push it to github, it will run the builds as we have already enabled our project to Travis CI.

#### Create Travis-CI configuration file 
- This configuration file tells Travis, what to do when we push some changes to github. Create `.travis.yml` file in the root directory ie., `project_root_dir`.
    ```console
    foo@bar:~/project_root_dir$ touch .travis.yml
    ```
- Now as usual, copy the contents of this file from our repo to the file that you have created. All the things that are there are self explanatory( sort of). The script that we run everytime we push something to github is ```docker-compose run app sh -c "python manage.py test && flake8"```. If this script exits with a failure, then it will fail the build and it send us the notification. We have already added flake8 in our requirements.txt file.

- let's create a configuration file ```.flake8``` in our ```project_name_in_local``` directory. This contains some django file names, which we will exclude so that it doesn't fail the linting. After creating the file, copy the contents from our repository to the file that you have created.
    ```console
    foo@bar:~/project_root_dir$ touch project_name_in_local/.flake8
    ```
- Let's just commit and push this to github. After this, the travis will run some builds and if it successful, then we did everything correctly.
     ```bash
    foo@bar:~/project_root_dir$ git add .
    foo@bar:~/project_root_dir$ git commit -m "Added Travis-CI and flake8 configuration"
    foo@bar:~/project_root_dir$ git push origin master
    ```
#### Verify Our installation process
- If everything is done properly, this should be our final file structure. And verify if build is successful or not in Travis-CI.

     ```
        project_root_dir
        └───project_name_in_local
        │   └───project_name
        │       │   __init__.py
        │       │   settings.py
        │       │   urls.py
        │       │   wsgi.py
        │       .flake8
        │       manage.py
        │   .gitignore
        │   .travis.yml
        │   docker-compose.yml
        |   Dockerfile
        |   LICENSE
        |   README.MD
        |   requirements.txt
    ```


License
----

MIT

credits : [Udemy Course by  Mark Winterbottom](https://www.udemy.com/django-python-advanced/) 
**Free Software, Hell Yeah!**