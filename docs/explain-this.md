# How this site works

Before we delve into how this works and the steps involved into recreating this, it's important to also know why.

## Why Create this?

This site was created as a personal profile to showcase some of not only the skills I possess, work with, wanted to highlight but also some skills that I wanted to learn and had been putting off. The project and it's entire codebase can also be cloned and used by others wanting to use this project for learning or just wanting to create a profile site to showcase their skills with some updates.

The creation of this site includes the use of but is not limited to the following skills:

- Python
- Markdown
- Yaml
- JSON
- Git
- Github
- Github Actions
- Mermaid Diagramming
- CI/CD Pipelines
- Github Pages
- MkDocs
- More


## Steps

### Design and Architecture

The first thing to understand would be the basic architecture of how the whole thing works.

The following diagram tries to outline the approach in a simple manner.


``` mermaid
architecture-beta
    group env(server)[Local Environment]
    service localrepo(server)[Python Venv with dependencies] in env
    service markdown(server)[Markdown and other Code] in env
    service git(database)[Local Git Repo] in env


    group github(cloud)[Github]
    service repository(database)[Repo] in github
    service action(server)[Actions] in github
    service deploy(server)[Pages] in github

    localrepo:R --> L:markdown
    markdown:R --> L:git

    repository:R --> L:action
    action:R --> L:deploy

    git{group}:R --> L:repository{group}

```

!!! TIP
    The diagram above was made with **Mermaid** which allows us to create visualizations using text only! Look at the markdown for this page and you won't find any images above!
    
    Want to learn more about it? Refer to this link [here](https://mermaid.js.org)


!!! IMPORTANT
    The above diagram is just to represent the workflow at a very high level and is not an accurate representation of everything that's involved.

### Implementation

#### Install python on your local Environment

This is a prerequisite and if you're here or if you run Linux as your main operating system, chances are that you have some version of python already installed. 

If not, head to the link [here](https://www.python.org/downloads/) for the installation instructions and more.

Once you're done, you should have python accessible in your environment. You can validated by using the following command:

```
python -V
```

This should show you the version installed.


#### Install dependencies

Now that you have python installed, you most likely also installed the package manager that comes along with it. 

You can check if that's the case using the command below:

```
pip -V
```

This should show you the version of pip installed.

If not, you can try installing it with:

```
python -m ensurepip --upgrade
```

Now we come down to the actual dependencies for this project but before that we need to set up a virtual environment.

#### Set up a virtual environment

There are several benefits to using a virtual environment instead of just the base python installation for your system. I'll skip those for the purpose of this tutorial but you can read all about it [here](https://www.geeksforgeeks.org/python/python-virtual-environment/).

The link above shows you how to set it up but the basic commands are below:

```
# Create a directory for your project
mkdir my_profile
cd my_profile
python -m venv venv
```

This creates a directory named venv inside your project directory and have all the necessary dependencies to run your project.


#### Install dependencies

We'll be using [MkDocs](https://www.mkdocs.org) to build our profile site from [Markdown](https://www.markdownguide.org).

!!! TIP
    Click on the links above to know about each of these technologies.

In simple terms, MKdocs take our Markdown files(Text) and creates static HTML pages out of them which you can then deploy to a Web server or in this case, **Github Pages**.

The following dependencies will be required at a minimum:

- mkdocs
- mkdocs-material
- neoteroi-mkdocs (required for timelines)

You can install the above using the command:

```
pip install mkdocs mkdocs-material neoteroi-mkdocs
```

You can use a different theme or more extensions if you'd like but you'll need to ensure the packages for those are installed as well. Refer to the documentation links above for more information on how to do that.

#### Start Writing

Now comes the hard part! Write all about your self.

This project contains certain sections for timelines. If you prefer them to be just text, you can simplify some of the markdown code.

#### Create a basic configuration

The main configuration file for mkdocs that defines the *Navigation*, *Theme*, *Plugins*, etc.  is a yaml file named **mkdocs.yml**

You'll need basic understanding of YAML to be able to create and edit the mkdocs.yaml file. A good place to start would be a [tutorial](https://www.tutorialspoint.com/yaml/index.htm) if you're not familiar with the syntax.

You can clone the configuration file from the [repository](https://github.com/karanpreetsingh1990/hire-karan) or create your own by going through the documentation.

Create a directory named docs inside your project directory to store all your markdown files.

Now start writing! This is the part that will be on you to create.


#### Test your site ( Optional )

This is an optional step but can be helpful if you want to check how your site looks or if the mmarkdown files are being rendered correctly.

MkDocs provides a command to render your project on a local server for this purpose.

Run the following command within the project directory:

```
mkdocs serve
```

By default, your site should be accessible on **http://127.0.0.1:8000/**

!!! TIP
    If you want to check how your site looks and are editing your markdown files or navigation. Instead of running the mkdocs serve command use 
    
    ```
    mkdocs serve --livereload
    ```
    
    This will watch your files for changes and reload the site dynamically making it much easier to edit and update.

#### Track your project through Git

Assuming that you've created the markdown files and a basic configuration and are ready to push things to Github, we now need to track all the files and their changes in a repository.

We'll be using [git](https://git-scm.com) to do the same.

If you've not heard or used git, I'll suggest you check a tutorial on git at this point and return here.

Within your project directory use the following steps to initialize, add and commit your code to a local repository.

```
# Initialize a git repository
$ git init
# Check the current status of the repository
$ git status
# Add all files to be tracked and staged 
$ git add . 
# Set the branch to be "main"
$ git branch -M main
# Commit your changes to the branch.
$ git commit -m "< Your Commit Message >"
```

#### Create a repository on Github

If you don't already have one, You'll need to create an accoung on Github at this point. You can register and login using your Google account if you're comfortable doing that or register seperately.

Once you're logged in, create a repository for your project. You can keep the visibility for this project Private for now but will need to make it public if you intend to use github pages to host the site.


#### Push your code to Github

The code we've commited to our local repository isn't very useful unless we want to store, build and host our site on the local environment only.

For now we'll push to Github and proceed with the next steps.

```
$ git add remote origin < Github Repository URL >
$ git push -u origin main
```

> You'll have to provide credentials for your repository to push your code.

#### Create Github Actions for CI/CD

Now that we have our code on Github, we'll need to build it so we get the static HTML from the markdown pages we created.

The commands to build mkdocs sites is as follows:

```
mkdocs build
```

This creates a site folder that has all your static pages.

We could do this on our local machine and manually deploy this to a webserver but since the goal is to automate this process with Github Actions, we'll do the following instead.

##### Set up github actions

You can do this step on Github directly as well but for our purpose, we'll do this on the local project directory. You can find more information on Github Actions [here](https://docs.github.com/en/actions/get-started/quickstart)

Create the following directory in your project and change the current working directory to it.

```
mkdir -p .github/workflows
cd .github/workflows
```

Create a file to store your workflows in this directory.

```
touch build-and-deploy.yaml
```

This file will store all the configuration that tells github actions to build and deploy our project.


##### Build your project

For this section, I won't dig deep into the code but will outline the steps needed. For more information on the steps and the exact code, refer to the github code.

To build our project we will:

1. Checkout our code to the runner.
2. Install project dependencies to the runner
3. Build our project
4. Upload the artifacts


##### Deploy your project

For this section, I won't dig deep into the code but will outline the steps needed. For more information on the steps and the exact code, refer to the github code.

To deploy our project we will:

1. Enable Github pages on our repository. For more information on how to do this, refer to the documentation [here](https://docs.github.com/en/pages/quickstart).
2. Deploy the built site to github pages.


!!! IMPORTANT
    To ensure you're able to host github pages on your repository, you'll have to make your repository public. If you created a private repository to begin with, you'll need to ensure you make it public before you can use Github Pages to host your profile.

!!! IMPORTANT
    Make sure you select the gh-pages branch under the Build and deployment settings for Pages to ensure your site is correctly deployed.

#### Access your project

Now that we've done everything, we need to be able to visit our site and marvel at our creation.

You can do that by going to the Pages section of settings in your repository and click on the **Visit Site** button.

Or you can build your URL using the following format:


```
https://< User Name >.github.io/< Repository Name >/
```



### That's All folks

The purpose of this page is to explain how it works and provide high level steps that allow you to follow and create something similar. This isn't meant to be copied as is but provides just enough information to get started and figure out things on your own instead.

It's to be used as a learning tool.

Everything that's needed for you to create your own profile is available in the repository.

Happy Trails!!
