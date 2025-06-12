---
layout: post
title: Tools Verify using Shell
description: Linux and the shell is used in this example to setup and verify the installation of the tools.  Additionally, a few programming exercises are included.
categories: ['DevOps']
permalink: /devops/tools/verify
menu: nav/tools_setup.html
toc: True
comments: True
---

## Computers and Terminals

A brief overview of Terminal and Linux is a step on your way to becoming a Linux expert. When a computer boots up, **a kernel (MacOS, Windows, Linux) is started**. This kernel is the core of the operating system and manages hardware resources. Above the kernel, various applications run, including the **shell** and **terminal**, which allow users to interact with the system using a basic set of commands provided by the kernel.

Typically, casual users interact with the system through a Desktop User Interface (UI) that is started by the computer's boot-up processes. However, to interact directly with the shell, users can run a "terminal" application through the Desktop UI. Additionally, **VS Code provides the ability to activate a "terminal"** within its editing environment, making it convenient for developers to execute commands without leaving the code editor.

In this next phase, we will use a Jupyter notebook to perform Linux commands through a terminal. The Jupyter notebook is an application that runs above the kernel, providing an interactive environment for writing and executing code, including shell commands. This setup allows us to seamlessly integrate code execution, data analysis, and documentation in one place, enhancing our productivity and learning experience.

## Setup a Personal GitHub Pages Project

You will be making a personal copy of the course repository. Be sure to have a GitHub account!!!

- Use the **Green "Use this Template"** button on the [portfolio_2025](https://github.com/nighthawkcoders/portfolio_2025) repository page to set up your personal GitHub Pages repository.
- Create a new repository.
- Fill in the dialog and select the **Repository Name** to be under your GitHub ID ownership.

    ![create repo]({{ site.baseurl }}/images/notebooks/foundation/create_repo.jpg)

- After this is complete, use the **Green "Code"** button on the newly created repository page to capture your "Project Repo" name.

In the next few code cells, we will run a bash (shell) script to pull a GitHub project. 

## Shell Script and Variables

We will ultimately run a bash (shell) script to pull a GitHub project. This next script simply sets up the necessary **environment variables** to tell the script the location of repository from GitHub and where to copy the output.

For now, focus on each line that begins with `export`. These are **shell variables**. Each line has a **name** (after the keyword `export`) and a **value** (after the equal sign).

Here is a full description:

- **Creates a temporary file** `/tmp/variables.sh` to store environment variables.
- **Sets the `project_dir` variable** to your home directory with a subdirectory named `nighthawk`. You can change `nighthawk` to a different name to test your git clone.
- **Sets the `project` variable** to a subdirectory within `project_dir` named `portfolio_2025`. You can change `portfolio_2025` to the name of your project.
- **Sets the `project_repo` variable** to the URL of the GitHub repository. Change this to the project you created from the `portfolio_2025` template.

**By running this script, you will prepare your environment for cloning** and working on your GitHub project. This is an essential step in setting up your development environment and ensuring that all dependencies are correctly configured.


```python
%%script bash

# Dependency Variables, set to match your project directories

cat <<EOF > /tmp/variables.sh
export project_dir=$HOME/nighthawk  # change nighthawk to different name to test your git clone
export project=\$project_dir/student_2025  # change student_2025 to name of project from git clone
export project_repo="https://github.com/nighthawkcoders/student_2025.git"  # change to project you created from portfolio_2025 template 
EOF
```

## Describing the Outputs of the Variables

The next script will extract the saved variables and display their values. Here is a description of the commands:

- The `source` command loads the variables that we saved in the `/tmp/variables.sh` file in the previous code cell.
- The `echo` commands display the contents of the named variables:
  - **project_dir**: The directory where your project is located.
  - **project**: The specific project directory within `project_dir`.
  - **project_repo**: The URL of the GitHub repository.

By running this script, you can verify that the environment variables are correctly set in your development environment. If they don't match up, go back to the previous code cell and make the necessary corrections.


```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

# Output shown title and value variables
echo "Project dir: $project_dir"
echo "Project: $project"
echo "Repo: $project_repo"
```

    Project dir: /home/kasm-user/nighthawk
    Project: /home/kasm-user/nighthawk/student_2025
    Repo: https://github.com/nighthawkcoders/student_2025.git


## Project Setup and Analysis with Bash Scripts
The bash scripts that follow automate what was done in the Tools Installation procedures with regards to cloning a GitHub project. Doing this in a script fashion adds the following benefits:

- After completing these steps, we will have notes on how to set up and verify a project.
- By reviewing these commands, you will start to learn the basics of Linux.
- By setting up these code cells, you will be learning how to develop automated scripts using Shell programming.
- You will learn that pretty much anything we type on a computer can be automated through the use of variables and a coding language.

### Pull Code

> Pull code from GitHub to your machine. This is a **bash script**, a sequence of commands, that will create a project directory and add the "project" from GitHub to the vscode directory. There is conditional logic to make sure that the clone only happens if it does not (!) exist. Here are some key elements in this code:

- `cd` command (change directory), remember this from the terminal session.
- `if` statements (conditional statements, called selection statements by College Board), code inside only happens if the condition is met.

Run the script two times and you will see that the output changes.  In the second run, the files exist and it impact the flow of the code.


```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Using conditional statement to create a project directory and project"

cd ~    # start in home directory

# Conditional block to make a project directory
if [ ! -d $project_dir ]
then 
    echo "Directory $project_dir does not exist... making directory $project_dir"
    mkdir -p $project_dir
fi
echo "Directory $project_dir exists." 

# Conditional block to git clone a project from project_repo
if [ ! -d $project ]
then
    echo "Directory $project does not exist... cloning $project_repo"
    cd $project_dir
    git clone $project_repo
    cd ~
fi
echo "Directory $project exists."
```

    Using conditional statement to create a project directory and project
    Directory /home/kasm-user/nighthawk exists.
    Directory /home/kasm-user/nighthawk/student_2025 exists.


### Look at Files in GitHub Project

> All computers contain files and directories. The clone brought more files from the cloud to your machine. Review the bash shell script, observe the commands that show and interact with files and directories. These were used during setup.

- `ls` lists computer files in Unix and Unix-like operating systems.
- `cd` offers a way to navigate and change the working directory.
- `pwd` prints the working directory.
- `echo` is used to display a line of text/string that is passed as an argument.


```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Navigate to project, then navigate to area wwhere files were cloned"
cd $project
pwd

echo ""
echo "list top level or root of files with project pulled from github"
ls

```

    Navigate to project, then navigate to area wwhere files were cloned
    /home/kasm-user/nighthawk/student_2025
    
    list top level or root of files with project pulled from github
    404.html
    assets
    _config.yml
    Gemfile
    Gemfile.lock
    images
    _includes
    index.md
    _layouts
    LICENSE
    Makefile
    navigation
    _notebooks
    _posts
    README4YML.md
    README.md
    requirements.txt
    _sass
    scripts
    _site
    venv


### Look at File List with Hidden and Long Attributes

> Most Linux commands have options to enhance behavior. The enhanced listing below shows permission bits, owner of the file, size, and date.

Some useful `ls` flags:
- `-a`: List all files including hidden files.
- `-l`: List in long format.
- `-h`: Human-readable file sizes.
- `-t`: Sort by modification time.
- `-R`: Reverse the order of the sort.

[ls reference](https://www.rapidtables.com/code/linux/ls.html)


```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Navigate to project, then navigate to area wwhere files were cloned"
cd $project
pwd

echo ""
echo "list all files in long format"
ls -al   # all files -a (hidden) in -l long listing
```

    Navigate to project, then navigate to area wwhere files were cloned
    /home/kasm-user/nighthawk/student_2025
    
    list all files in long format
    total 132
    drwxr-xr-x 16 kasm-user kasm-user  4096 Nov 18 15:25 .
    drwxr-xr-x  3 kasm-user kasm-user  4096 Nov 18 15:12 ..
    -rw-r--r--  1 kasm-user kasm-user   436 Nov 18 15:12 404.html
    drwxr-xr-x  5 kasm-user kasm-user  4096 Nov 18 15:12 assets
    -rw-r--r--  1 kasm-user kasm-user   839 Nov 18 15:12 _config.yml
    -rw-r--r--  1 kasm-user kasm-user   122 Nov 18 15:12 Gemfile
    -rw-r--r--  1 kasm-user kasm-user  7927 Nov 18 15:20 Gemfile.lock
    drwxr-xr-x  8 kasm-user kasm-user  4096 Dec 20 21:40 .git
    drwxr-xr-x  3 kasm-user kasm-user  4096 Nov 18 15:12 .github
    -rw-r--r--  1 kasm-user kasm-user   251 Nov 18 15:12 .gitignore
    drwxr-xr-x  3 kasm-user kasm-user  4096 Nov 18 15:12 images
    drwxr-xr-x  4 kasm-user kasm-user  4096 Nov 18 15:12 _includes
    -rw-r--r--  1 kasm-user kasm-user   101 Nov 18 15:12 index.md
    drwxr-xr-x  2 kasm-user kasm-user  4096 Nov 18 15:12 _layouts
    -rw-r--r--  1 kasm-user kasm-user 11357 Nov 18 15:12 LICENSE
    -rw-r--r--  1 kasm-user kasm-user  3549 Nov 18 15:12 Makefile
    drwxr-xr-x  2 kasm-user kasm-user  4096 Nov 18 15:12 navigation
    drwxr-xr-x  3 kasm-user kasm-user  4096 Nov 18 15:12 _notebooks
    drwxr-xr-x  3 kasm-user kasm-user  4096 Nov 18 15:12 _posts
    -rw-r--r--  1 kasm-user kasm-user    79 Nov 18 15:12 README4YML.md
    -rw-r--r--  1 kasm-user kasm-user 14171 Nov 18 15:12 README.md
    -rw-r--r--  1 kasm-user kasm-user    57 Nov 18 15:12 requirements.txt
    drwxr-xr-x  4 kasm-user kasm-user  4096 Nov 18 15:12 _sass
    drwxr-xr-x  4 kasm-user kasm-user  4096 Nov 18 15:25 scripts
    drwxr-xr-x 12 kasm-user kasm-user  4096 Dec 20 21:48 _site
    drwxr-xr-x  7 kasm-user kasm-user  4096 Nov 18 15:20 venv
    drwxr-xr-x  2 kasm-user kasm-user  4096 Nov 18 15:12 .vscode



```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Look for posts"
export posts=$project/_posts  # _posts inside project
cd $posts  # this should exist per fastpages
pwd  # present working directory
ls -lR  # list posts recursively
```

    Look for posts
    /home/kasm-user/nighthawk/student_2025/_posts
    .:
    total 4
    drwxr-xr-x 4 kasm-user kasm-user 4096 Nov 18 15:25 Foundation
    
    ./Foundation:
    total 12
    -rw-r--r-- 1 kasm-user kasm-user 2683 Nov 18 15:25 2024-08-21-sprint1_plan_IPYNB_2_.md
    drwxr-xr-x 2 kasm-user kasm-user 4096 Nov 18 15:25 A-pair_programming
    drwxr-xr-x 2 kasm-user kasm-user 4096 Nov 18 15:25 B-tools_and_equipment
    
    ./Foundation/A-pair_programming:
    total 24
    -rw-r--r-- 1 kasm-user kasm-user  5433 Nov 18 15:12 2023-08-16-pair_programming.md
    -rw-r--r-- 1 kasm-user kasm-user  3242 Nov 18 15:25 2023-08-16-pair_showcase_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user 10043 Nov 18 15:25 2023-08-17-pair_habits_IPYNB_2_.md
    
    ./Foundation/B-tools_and_equipment:
    total 92
    -rw-r--r-- 1 kasm-user kasm-user  8196 Nov 18 15:25 2023-08-19-devops_accounts_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user  4949 Nov 18 15:25 2023-08-21-devops_tools-home_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user 17367 Nov 18 15:25 2023-08-21-devops_tools-setup_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user 16130 Nov 18 15:25 2023-08-22-devops_tools-verify_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user 25977 Nov 18 15:25 2023-08-23-devops-githhub_pages-play_IPYNB_2_.md
    -rw-r--r-- 1 kasm-user kasm-user  7506 Nov 18 15:25 2023-08-23-devops-hacks_IPYNB_2_.md



```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Look for notebooks"
export notebooks=$project/_notebooks  # _notebooks is inside project
cd $notebooks   # this should exist per fastpages
pwd  # present working directory
ls -lR  # list notebooks recursively
```

    Look for notebooks
    /home/kasm-user/nighthawk/student_2025/_notebooks
    .:
    total 4
    drwxr-xr-x 4 kasm-user kasm-user 4096 Nov 18 15:12 Foundation
    
    ./Foundation:
    total 12
    -rw-r--r-- 1 kasm-user kasm-user 3509 Nov 18 15:12 2024-08-21-sprint1_plan.ipynb
    drwxr-xr-x 2 kasm-user kasm-user 4096 Nov 18 15:12 A-pair_programming
    drwxr-xr-x 2 kasm-user kasm-user 4096 Nov 18 15:12 B-tools_and_equipment
    
    ./Foundation/A-pair_programming:
    total 16
    -rw-r--r-- 1 kasm-user kasm-user  3918 Nov 18 15:12 2023-08-16-pair_showcase.ipynb
    -rw-r--r-- 1 kasm-user kasm-user 11624 Nov 18 15:12 2023-08-17-pair_habits.ipynb
    
    ./Foundation/B-tools_and_equipment:
    total 112
    -rw-r--r-- 1 kasm-user kasm-user  9767 Nov 18 15:12 2023-08-19-devops_accounts.ipynb
    -rw-r--r-- 1 kasm-user kasm-user  5931 Nov 18 15:12 2023-08-21-devops_tools-home.ipynb
    -rw-r--r-- 1 kasm-user kasm-user 22859 Nov 18 15:12 2023-08-21-devops_tools-setup.ipynb
    -rw-r--r-- 1 kasm-user kasm-user 23150 Nov 18 15:12 2023-08-22-devops_tools-verify.ipynb
    -rw-r--r-- 1 kasm-user kasm-user 32309 Nov 18 15:12 2023-08-23-devops-githhub_pages-play.ipynb
    -rw-r--r-- 1 kasm-user kasm-user  9478 Nov 18 15:12 2023-08-23-devops-hacks.ipynb



```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Look for images, print working directory, list files"
cd $project/images  # this should exist per fastpages
pwd
ls -lR
```

    Look for images, print working directory, list files
    /home/kasm-user/nighthawk/student_2025/images
    .:
    total 56
    -rw-r--r-- 1 kasm-user kasm-user 15406 Nov 18 15:12 favicon.ico
    -rw-r--r-- 1 kasm-user kasm-user 34239 Nov 18 15:12 logo.png
    drwxr-xr-x 3 kasm-user kasm-user  4096 Nov 18 15:12 notebooks
    
    ./notebooks:
    total 4
    drwxr-xr-x 2 kasm-user kasm-user 4096 Nov 18 15:12 foundation
    
    ./notebooks/foundation:
    total 364
    -rw-r--r-- 1 kasm-user kasm-user 310743 Nov 18 15:12 create_repo.png
    -rw-r--r-- 1 kasm-user kasm-user  29416 Nov 18 15:12 push.jpg
    -rw-r--r-- 1 kasm-user kasm-user  17105 Nov 18 15:12 stage.jpg
    -rw-r--r-- 1 kasm-user kasm-user   6659 Nov 18 15:12 wsl.jpg


### Look inside a Markdown File
> "cat" reads data from the file and gives its content as output


```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

echo "Navigate to project, then navigate to area wwhere files were cloned"

cd $project
echo "show the contents of README.md"
echo ""

cat README.md  # show contents of file, in this case markdown
echo ""
echo "end of README.md"

```

    Navigate to project, then navigate to area wwhere files were cloned
    show the contents of README.md
    
    # Introduction
    
    Nighthawk Pages is a project designed to support students in their Computer Science and Software Engineering education. It offers a wide range of resources including tech talks, code examples, and educational blogs.
    
    GitHub Pages can be customized by the blogger to support computer science learnings as the student works through the pathway of using Javascript, Python/Flask, Java/Spring.  
    
    ## Student Requirements
    
    Del Norte HS students will be required to review their personal GitHub Pages at each midterm and final.  This review will contain a compilation of personal work performed within each significant grading period.
    
    In general, Students and Teachers are expected to use GitHub pages to build lessons, complete classroom hacks, perform work on JavaScript games, and serve as a frontend to full-stack applications.
    
    Exchange of information could be:
    
    1. sharing a file:  `wget "raw-link.ipynb"
    2. creating a template from this repository
    3. sharing a fork among team members
    4. etc.
    
    ---
    
    ## History
    
    This project is in its 3rd revision (aka 3.0).
    
    The project was initially based on Fastpages. But this project has diverged from those roots into an independent entity.  The decision to separate from Fastpages was influenced by the deprecation of Fastpages by authors.  It is believed by our community that the authors of fastpages turned toward Quatro.  After that change of direction fastpages did not align with the Teacher's goals and student needs. The Nighthawk Pages project has more of a raw development blogging need.
    
    ### License
    
    The Apache license has its roots in Fastpages.  Thus, it carries its license forward.  However, most of the code is likely unrecognizable from those roots.
    
    ### Key Features
    
    - **Code Examples**: Provides practical coding examples in JavaScript, including a platformer game, and frontend code for user databases using Python and Java backends.
    - **Educational Blogs**: Offers instructional content on various topics such as developer tools setup, deployment on AWS, SQL databases, machine learning, and data structures. It utilizes Jupyter Notebooks for interactive lessons and coding challenges.
    - **Tools and Integrations**: Features GitHub actions for blog publishing, Utterances for blog commenting, local development support via Makefile and scripts, and styling with the Minima Theme and SASS. It also includes a new integration with GitHub Projects and Issues.
    
    ### Contributions
    
    - **Notable Contributions**: Highlights significant contributions to the project, including theme development, search and tagging functionality, GitHub API integration, and the incorporation of GitHub Projects into GitHub pages. Contributors such as Tirth Thakker, Mirza Beg, and Toby Ledder have played crucial roles in these developments.
    
    - **Blog Contributions**:  Often students contribute articles and blogs to this project.  Their names are typically listed in the front matter of their contributing post.
    
    ---
    
    ## GitHub Pages setup
    
    The absolutes in setup up...
    
    **Activate GitHub Pages Actions**: This step involves enabling GitHub Pages Actions for your project. By doing so, your project will be automatically deployed using GitHub Pages Actions, ensuring that your project is always up to date with the latest changes you push to your repository.
    
    - On the GitHub website for the repository go to the menu: Settings -> Pages ->Build
    - Under the Deployment location on the page, select "GitHub Actions".
    
    **Update `_config.yml`**: You need to modify the `_config.yml` file to reflect your repository's name. This configuration is crucial because it ensures that your project's styling is correctly applied, making your deployed site look as intended rather than unstyled or broken.
    
    ```text
    github_repo: "student_2025" 
    baseurl: "/student_2025"
    ```
    
    **Set Repository Name in Makefile**: Adjust the `REPO_NAME` variable in your Makefile to match your GitHub repository's name. This action facilitates the automatic updating of posts and notebooks on your local development server, improving the development process.
    
    ```make
    # Configuration, override port with usage: make PORT=4200
    PORT ?= 4100
    REPO_NAME ?= student_2025
    LOG_FILE = /tmp/jekyll$(PORT).log
    ```
    
    ### Tool requirements
    
    All `GitHub Pages` websites are managed on GitHub infrastructure and use GitHub version control.  Each time we change files in GitHub it initiates a GitHub Action, a continuous integration and development toolset, that rebuilds and publishes the site with Jekyll.  
    
    - GitHub uses `Jekyll` to transform your markdown and HTML content into static websites and blogs. [Jekyll](https://jekyllrb.com/).
    - A Linux shell is required to work with this project integration with GitHub Pages, GitHub and VSCode.  Ubuntu is the preferred shell, though MacOS shell is supported as well.  There will be some key setup scripts that follow in the README.
    - Visual Studio Code is the Nighthawk Pages author's preferred code editor and extensible development environment.  VSCode has a rich ecosystem of developer extensions that ease working with GitHub Pages, GitHub, and many programming languages.  Setting up VSCode and extensions will be elaborated upon in this document.
    - An anatomy section in this README will describe GitHub Pages and conventions that are used to organize content and files.  This includes file names, key coding files, metadata tagging of blogs, styling tooling for blogs, etc.
    
    ### Development Environment Setup
    
    Comprehensive start. A topic-by-topic guide to getting this project running is published [here](https://nighthawkcoders.github.io/portfolio_2025/devops/tools/home).
    
    Quick start.  A quick start below is a reminder, but is dependent on your knowledge.  Only follow this instruction if you need a refresher.  Always default to the comprehensive start if any problem occurs.
    
    #### Clone Repo
    
    Run these commands to obtain the project, then locate into the project directory with the terminal, install an extensive set of tools, and make.
    
    ```bash
    git clone <this-repo> # git clone https://github.com/nighthawkcoders/student_2025.git 
    cd <repo-dir>/scripts # cd student_2025
    ```
    
    #### Windows WSL and/or Ubuntu Users
    
    - Execute the script: `./activate_ubuntu.sh`
    
    #### macOS Users
    
    - Execute the script: `./activate_macos.sh`
    
    #### Kasm Cloud Desktop Users
    
    - Execute the script: `./activate.sh`
    
    ## Run Server on localhost
    
    To preview the project you will need to "make" the project.
    
    ### Bundle install
    
    The very first time you clone run project you will need to run this Ruby command as the final part of your setup.
    
    ```bash
    bundle install
    ```
    
    ### Start the Server  
    
    This requires running terminal commands `make`, `make stop`, `make clean`, or `make convert` to manage the running server.  Logging of details will appear in the terminal.   A `Makefile` has been created in the project to support commands and start processes.
    
    Start the server, this is the best choice for initial and iterative development.  Note. after the initial `make`, you should see files automatically refresh in the terminal on VSCode save.
    
      ```bash
      make
      ```
    
    ### Load web application into the Browser
    
    Start the preview server in the terminal,
    The terminal output from `make` shows the server address. "Cmd" or "Ctl" click the http location to open the preview server in a browser. Here is an example Server address message, click on the Server address to load:...
    
      ```text
      http://0.0.0.0:4100/student_2025/
      ```
    
    ### Regeneration of web application
    
    Save on ".ipynb" or ".md" file activiates "regeneration". An example terminal message is below.  Refresh the browser to see updates after the message displays.
    
      ```text
      Regenerating: 1 file(s) changed at 2023-07-31 06:54:32
          _notebooks/2024-01-04-cockpit-setup.ipynb
      ```
    
    ### Other "make" commands
    
    Terminal messages are generated from background processes.  At any time, click return or enter in a terminal window to obtain a prompt.  Once you have the prompt you can use the terminal as needed for other tasks.  Always return to the root of project `cd ~/vscode/student_2025` for all "make" actions.
    
    #### Stop the preview server
    
    Stopping the server ends the web server applications running process.  However, it leaves constructed files in the project in a ready state for the next time you run `make`.
    
      ```bash
      make stop
      ```
    
    ### Clean the local web application environment
    
    This command will top the server and "clean" all previously constructed files (ie .ipynb -> .md). This is the best choice when renaming files has created duplicates that are visible when previewing work.
    
      ```bash
      make clean
      ```
    
    ### Observe build errors
    
    Test Jupyter Notebook conversions (ie .ipynb -> .md), this is the best choice to see if an IPYNB conversion error is occurring.
    
      ```bash
      make convert
      ```
    
    ---
    
    ## Development Support
    
    ### File Names in "_posts", "_notebooks"
    
    There are two primary directories for creating blogs.  The "_posts" directory is for authoring in markdown only.  The "_notebooks" allows for markdown, pythons, javascript and more.
    
    To name a file, use the following structure (If dates are in the future, review your config.yml setting if you want them to be viewed).  Review these naming conventions.
    
    - For markdown files in _posts:
      - year-month-day-fileName.md
        - GOOD EXAMPLE: 2021-08-02-First-Day.md
        - BAD EXAMPLE: 2021-8-2-first-day.md
        - BAD EXAMPLE: first-day.md
        - BAD EXAMPLE: 2069-12-31-First-Day.md
    
    - For Jupyter notebooks in _notebooks:
      - year-month-day-fileName.ipynb
        - GOOD EXAMPLE: 2021-08-02-First-Day.ipynb
        - BAD EXAMPLE: 2021-8-2-first-day.ipynb
        - BAD EXAMPLE: first-day.ipynb
        - BAD EXAMPLE: 2069-12-31-First-Day.ipynb
    
    ### Tags
    
    Tags are used to organize pages by their tag the way to add tags is to add the following to your front matter such as the example seen here `categories: [Tools]` Each item in the same category will be lumped together to be seen easily on the search page.
    
    ### Search
    
    All pages can be searched for using the built-in search bar. This search bar will search for any word in the title of a page or in the page itself. This allows for easily finding pages and information that you are looking for. However, sometimes this may not be desirable so to hide a page from the search you need to add `search_exclude: true` to the front matter of the page. This will hide the page from appearing when the viewer uses search.
    
    ### Navigation Bar
    
    To add pages to the top navigation bar use _config.yml to order and determine which menus you want and how to order them.  Review the_config.yml in this project for an example.
    
    ### Blog Page
    
    There is a blog page that has options for images and a description of the page. This page can help the viewer understand what the page is about and what they can expect to find on the page. The way to add images to a page is to have the following front matter `image: /images/file.jpg` and then the name of the image that you want to use. The image must be in the `images` folder. Furthermore, if you would like the file to not show up on the blog page `hide: true` can be added to the front matter.
    
    ### SASS support
    
    NIGHTHAWK Pages support a variety of different themes that are each overlaid on top of minima. To use each theme, go to the "_sass/minima/custom-styles.scss" file and simply comment or uncomment the theme you want to use.
    
    To learn about the minima themes search for "GitHub Pages minima" and review the README.
    
    To find a new theme search for "Github Pages Themes".
    
    ### Includes
    
    - Nighthawk Pages uses liquid syntax to import many common page elements that are present throughout the repository. These common elements are imported from the _includes directory. If you want to add one of these common elements, use liquid syntax to import the desired element to your file. Here’s an example of the liquid syntax used to import: `{%- include post_list.html -%}` Note that the liquid syntax is surrounded by curly braces and percent signs. This can be used anywhere in the repository.
    
    ### Layouts
    
    - To use or create a custom page layout, make an HTML page inside the _layouts directory, and when you want to use that layout in a file, use the following front matter `layout: [your layout here]`.  All layouts will be written in liquid to define the structure of the page.
    
    ### Metadata
    
    Metadata, also known as "front matter", is a set of key-value pairs that can provide additional information to GitHub Pages about .md and .ipynb files. This can and probably will be used in other file types (ie doc, pdf) if we add them to the system.
    
    In the front matter, you can also define things like a title and description for the page.  Additional front matter is defined to place content on the "Computer Science Lab Notebook" page.  The `courses:` key will place data on a specific page with the nested `week:` placing data on a specific row on the page.  The `type:` key in "front matter" will place the blog under the plans, hacks(ToDo), and tangibles columns.
    
    - In our files, the front matter is defined at the top of the page or the first markdown cell.
    
      - First, open one of the .md or .ipynb files already included in either your _posts|_notebooks folder.
    
      - In the .md file, you should notice something similar to this at the top of the page. To see this in your .ipynb files you will need to double-click the markdown cell at the top of the file.
    
      ```yaml
      ---
      toc: true
      comments: true
      layout: post
      title: Jupyter Python Sample
      description: Example Blog!!!  This shows code and notes from hacks.
      type: ccc
      courses: { csa: {week: 5} }
      ---
      ```
    
    - The front matter will always have '---' at the top and bottom to distinguish it and each key-value pair will be separated by a ':'.
    
    - Here we can modify things like the title and description.
    
    - The type value will tell us which column this is going to appear under the time box supported pages.  The "ccc" stands for Code, Code, Code.
    
    - The courses will tell us which menu item it will be under, in this case, the `csa` menu, and the `week` tells it what row (week) it will appear under that menu.
    
    end of README.md


## Env, Git, and GitHub

> Env(ironment) is used to capture things like the path to the Code or Home directory. Git and GitHub are not only used to exchange code between individuals but are also often used to exchange code through servers, in our case for website deployment. All tools we use have behind-the-scenes relationships with the system they run on (MacOS, Windows, Linux) or a relationship with servers to which they are connected (e.g., GitHub). There is an "env" command in bash. There are environment files and setting files (e.g., `.git/config`) for Git. They both use a key/value concept.

- `env` shows settings for your shell.
- `git clone` sets up a directory of files.
- `cd $project` allows the user to move inside that directory of files.
- `.git` is a hidden directory that is used by Git to establish a relationship between the machine and the Git server on GitHub.


```python
%%script bash

# This command has no dependencies

echo "Show the shell environment variables, key on left of equal value on right"
echo ""

env
```

    Show the shell environment variables, key on left of equal value on right
    
    SHELL=/bin/bash
    SESSION_MANAGER=local/kasm:@/tmp/.ICE-unix/1964,unix/kasm:/tmp/.ICE-unix/1964
    WINDOWID=31457283
    VNCOPTIONS=-PreferBandwidth -DynamicQualityMin=4 -DynamicQualityMax=7 -DLP_ClipDelay=0 -select-de manual -UnixRelay printer:/tmp/printer
    KASM_API_HOST=172.31.42.43
    COLORTERM=truecolor
    XDG_CONFIG_DIRS=/etc/xdg
    PYTHONUNBUFFERED=1
    XDG_MENU_PREFIX=xfce-
    APPLICATION_INSIGHTS_NO_DIAGNOSTIC_CHANNEL=1
    PULSE_RUNTIME_PATH=/var/run/pulse
    HOSTNAME=kasm
    LANGUAGE=en_US.UTF-8
    SDL_GAMECONTROLLERCONFIG=030000005e040000be02000014010000,XInput Controller,platform:Linux,a:b0,b:b1,x:b2,y:b3,back:b8,guide:b16,start:b9,leftstick:b10,rightstick:b11,leftshoulder:b4,rightshoulder:b5,dpup:b12,dpdown:b13,dpleft:b14,dpright:b15,leftx:a0,lefty:a1,rightx:a2,righty:a3,lefttrigger:b6,righttrigger:b7
    KASM_SVC_SEND_CUT_TEXT=-SendCutText 1
    DISTRO=ubuntu
    INST_DIR=/dockerstartup/install
    JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
    SSH_AUTH_SOCK=/tmp/ssh-XXXXXXdcQWI8/agent.2177
    PYTHON_FROZEN_MODULES=on
    ELECTRON_RUN_AS_NODE=1
    START_PULSEAUDIO=1
    KASM_PROFILE_LDR=1
    DESKTOP_SESSION=xfce
    SSH_AGENT_PID=2181
    KASM_SVC_AUDIO_INPUT=1
    NO_AT_BRIDGE=1
    KASM_SVC_AUDIO=1
    PWD=/home/kasm-user/nighthawk/student_2025/_notebooks/Foundation/B-tools_and_equipment
    VNC_RESOLUTION=1698x912
    KASM_ID=fc7639cf-0f91-4155-9c09-56d0e188e930
    KASM_SVC_RDP_CLIENT_FILE_TRANSFER=0
    PANEL_GDK_CORE_DEVICE_EVENTS=0
    NVIDIA_DRIVER_CAPABILITIES=graphics,compat32,utility
    VSCODE_ESM_ENTRYPOINT=vs/workbench/api/node/extensionHostProcess
    PYDEVD_IPYTHON_COMPATIBLE_DEBUGGING=1
    VSCODE_CODE_CACHE_PATH=/home/kasm-user/.config/Code/CachedData/fabdb6a30b49f79a7aba0f2ad9df9b399473380f
    KASM_SVC_DOWNLOADS=1
    TZ=America/Los_Angeles
    OMP_WAIT_POLICY=PASSIVE
    HOME=/home/kasm-user
    LANG=en_US.UTF-8
    XDG_CURRENT_DESKTOP=XFCE
    AUDIO_PORT=4901
    VIRTUAL_ENV=/home/kasm-user/nighthawk/student_2025/venv
    VSCODE_IPC_HOOK=/home/kasm-user/.config/Code/1.96-main.sock
    VTE_VERSION=6800
    DONT_PROMPT_WSL_INSTALL=No_Prompt_please
    STARTUPDIR=/dockerstartup
    FORCE_COLOR=1
    VSCODE_CLI=1
    PYDEVD_USE_FRAME_EVAL=NO
    CLICOLOR=1
    VSCODE_L10N_BUNDLE_LOCATION=
    PROXYPATH=9c31ebd1-e8c6-4d/3de54473-8cf1-45aa-ab8c-26796d43ba9e
    SKIP_CLEAN=true
    CLICOLOR_FORCE=1
    CHROME_DESKTOP=code.desktop
    KASM_SVC_GAMEPAD=0
    KASM_SVC_WEBCAM=0
    GEM_HOME=/home/kasm-user/gems
    KASM_SVC_RECORDER=0
    TERM=xterm-color
    GOMP_SPINCOUNT=0
    GIT_PAGER=cat
    KASMVNC_AUTO_RECOVER=true
    VNC_COL_DEPTH=24
    KASM_SVC_UPLOADS=1
    PYTHONIOENCODING=utf-8
    KASM_SVC_PRINTER=1
    START_XFCE4=1
    DISPLAY=:1.0
    KASM_API_JWT=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJrYXNtX2lkIjoiZmM3NjM5Y2YtMGY5MS00MTU1LTljMDktNTZkMGUxODhlOTMwIiwicHJvZmlsZV9wYXRoIjoiczM6Ly9rYXNtLXByb2ZpbGUvY3NhLXVidW50dS9lYTI1NTI3ZC0xZjQ0LTRhZGYtOTZkYy02NjkzNDY2NGRhNWEvam0xMDIxLyIsImV4cCI6MTgyOTM2NzU3MywiYXV0aG9yaXphdGlvbnMiOls1MF19.PpCwabMjW-jKyPSW-qWmuvRI3EzVK8phlSpjc7aR9jBiXfNM8c2L_qR4TtbYBjZ6uiTw38CQTl2qcGJ03wtagnt5WC8dOHylvz65TPk2AuyDa2Beg70zqpZYesVWGzJaCvE4E83lCKYG6a3H01YarJTly2H-RZvEhdd1qEXC2T4et7GMCrm3VnflD6HgC4oq41eeg4_SjOZLHmoeOOkz2pap3GgCNO-rM9d8aX9UuwqzFCfxqPwLvbBqIRp6-DJ9Yetr1Ccz7Pg2JISTBsTLajQ7F0dJRIKRh0u9V10-bIu4Y64w5mc0fWofl3oW-KozmzNt9KZlepzoaMSM2rK-Nw
    VSCODE_PID=5794
    SHLVL=2
    PAGER=cat
    MAX_FRAME_RATE=24
    APPLICATIONINSIGHTS_CONFIGURATION_CONTENT={}
    VSCODE_CWD=/home/kasm-user/nighthawk/student_2025
    VIRTUAL_ENV_PROMPT=(venv) 
    KASM_VNC_PATH=/usr/share/kasmvnc
    MPLBACKEND=module://matplotlib_inline.backend_inline
    KASM_RX_HOME=/dockerstartup/kasmrx
    LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:/usr/lib/i386-linux-gnu:/usr/local/nvidia/lib:/usr/local/nvidia/lib64
    VSCODE_CRASH_REPORTER_PROCESS_TYPE=extensionHost
    LC_ALL=en_US.UTF-8
    KASM_SVC_ACCEPT_CUT_TEXT=-AcceptCutText 1
    ELECTRON_NO_ATTACH_CONSOLE=1
    INST_SCRIPTS=/ubuntu/install/tools/install_tools.sh                   /ubuntu/install/chrome/install_chrome.sh                   /ubuntu/install/chromium/install_chromium.sh                   /ubuntu/install/sublime_text/install_sublime_text.sh                   /ubuntu/install/slack/install_slack.sh                   /ubuntu/install/vs_code/install_vs_code.sh                   /ubuntu/install/postman/install_postman.sh                   /ubuntu/install/cleanup/cleanup.sh
    XDG_DATA_DIRS=/usr/local/share:/usr/share
    GDK_BACKEND=x11
    VNC_PORT=5901
    PATH=/home/kasm-user/nighthawk/student_2025/venv/bin:/home/kasm-user/nighthawk/student_2025/venv/bin:/home/kasm-user/gems/bin:/home/kasm-user/gems/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/jvm/java-17-openjdk-amd64/bin:/usr/lib/jvm/java-17-openjdk-amd64/bin:/usr/lib/jvm/java-17-openjdk-amd64/bin:/usr/lib/jvm/java-17-openjdk-amd64/bin
    NO_VNC_PORT=6901
    ORIGINAL_XDG_CURRENT_DESKTOP=XFCE
    DBUS_SESSION_BUS_ADDRESS=unix:abstract=/tmp/dbus-95lbTjKEHY,guid=ff5f42bc923ee0d8cac82336676654cb
    VSCODE_NLS_CONFIG={"userLocale":"en-us","osLocale":"en-us","resolvedLanguage":"en","defaultMessagesFile":"/usr/share/code/resources/app/out/nls.messages.json","locale":"en-us","availableLanguages":{}}
    DEBIAN_FRONTEND=noninteractive
    VSCODE_HANDLES_UNCAUGHT_ERRORS=true
    OLDPWD=/home/kasm-user/nighthawk
    KASM_API_PORT=443
    _=/usr/bin/env



```python
%%script bash

# Extract saved variables
source /tmp/variables.sh

cd $project

echo ""
echo "show the secrets of .git config file"
cd .git
ls -l config

echo ""
echo "look at config file"
cat config
```

    
    show the secrets of .git config file
    -rw-r--r-- 1 kasm-user kasm-user 305 Nov 18 15:21 config
    
    look at config file
    [core]
    	repositoryformatversion = 0
    	filemode = true
    	bare = false
    	logallrefupdates = true
    [remote "origin"]
    	url = https://github.com/nighthawkcoders/student_2025.git
    	fetch = +refs/heads/*:refs/remotes/origin/*
    [branch "main"]
    	remote = origin
    	merge = refs/heads/main
    	vscode-merge-base = origin/main


## Advanced Shell project

> This example was requested by a student (Jun Lim, CSA). The request was to make a Jupyter file using bash; I adapted the request to markdown. This type of thought will have great extrapolation to coding and possibilities of using Lists, Arrays, or APIs to build user interfaces. JavaScript is a language where building HTML is very common.

> To get more interesting output from the terminal, this will require using something like mdless (https://github.com/ttscoff/mdless). This enables seeing markdown in rendered format.
- On Desktop [Install PKG from MacPorts](https://www.macports.org/install.php)
- In Terminal on MacOS
    - [Install ncurses](https://ports.macports.org/port/ncurses/)
    - ```gem install mdless```
- In Terminal on Ubuntu
    - ```gem install mdless```

This is starting the process of documentation.


```python
%%script bash

# This example has an error in VSCode; it runs best on Jupyter
cd /tmp

file="sample.md"
if [ -f "$file" ]; then
    rm $file
fi

# Create a markdown file using tee and here document (<<EOF)
tee -a $file >/dev/null <<EOF
# Show Generated Markdown
This introductory paragraph and this line and the title above are generated using tee with the standard input (<<) redirection operator.
- This bulleted element is still part of the tee body.
EOF

# Append additional lines to the markdown file using echo and redirection (>>)
echo "- This bulleted element and lines below are generated using echo with standard output (>>) redirection operator." >> $file
echo "- The list definition, as is, is using space to separate lines. Thus the use of commas and hyphens in output." >> $file

# Define an array of actions and their descriptions
actions=("ls,list-directory" "cd,change-directory" "pwd,present-working-directory" "if-then-fi,test-condition" "env,bash-environment-variables" "cat,view-file-contents" "tee,write-to-output" "echo,display-content-of-string" "echo_text_>\$file,write-content-to-file" "echo_text_>>\$file,append-content-to-file")

# Loop through the actions array and append each action to the markdown file
for action in ${actions[@]}; do
  action=${action//-/ }  # Convert dash to space
  action=${action//,/: } # Convert comma to colon
  action=${action//_text_/ \"sample text\" } # Convert _text_ to "sample text", note escape character \ to avoid "" having meaning
  echo "    - ${action//-/ }" >> $file  # Append action to file
done

echo ""
echo "File listing and status"
ls -l $file # List file details
wc $file   # Show word count
mdless $file  # Render markdown from terminal (requires mdless installation)

rm $file  # Clean up temporary file
```

    
    File listing and status
    -rw-r--r-- 1 kasm-user kasm-user 808 Dec 20 21:55 sample.md
     15 132 808 sample.md


    Config file saved to /home/kasm-user/.config/mdless/config.yml


    
    
    [0;37m[0;1;90;47mShow Generated Markdown [0;2;30;47m======================================================[0;37m
    
    [0;37mThis introductory paragraph and this line and the title above are generated using tee with the standard input (<<) redirection operator.[0;37m
    
     [0;1;91m* [0;97mThis bulleted element is still part of the tee body.[0;37m
     [0;1;91m* [0;97mThis bulleted element and lines below are generated using echo with standard output (>>) redirection operator.[0;37m
     [0;1;91m* [0;97mThe list definition, as is, is using space to separate lines. Thus the use of commas and hyphens in output.
       [0;1;91m* [0;97mls: list directory[0;97m
       [0;1;91m* [0;97mcd: change directory[0;97m
       [0;1;91m* [0;97mpwd: present working directory[0;97m
       [0;1;91m* [0;97mif then fi: test condition[0;97m
       [0;1;91m* [0;97menv: bash environment variables[0;97m
       [0;1;91m* [0;97mcat: view file contents[0;97m
       [0;1;91m* [0;97mtee: write to output[0;97m
       [0;1;91m* [0;97mecho: display content of string[0;97m
       [0;1;91m* [0;97mecho "sample text" >$file: write content to file[0;97m
       [0;1;91m* [0;97mecho "sample text" >>$file: append content to file[0;97m[0;37m
    
    [0m

## Display Shell commands help using `man`

The `man` command is not supported on KASM workspace Ubuntu.

> The previous example used a markdown file to store a list of actions and their descriptions. This example uses the `man` command to generate a markdown file with descriptions of the commands. The markdown file is then displayed using `mdless`.

In coding, we should try to get data from the content creators instead of creating it on our own. This approach has several benefits:
- **Accuracy**: Descriptions from `man` pages are authoritative and accurate, as they come directly from the documentation provided by the command's developers.
- **Consistency**: Automatically generating descriptions ensures consistency in formatting and terminology.
- **Efficiency**: It saves time and effort, especially when dealing with a large number of commands.
- **Up-to-date Information**: `man` pages are regularly updated with the latest information, ensuring that the descriptions are current.


```python
%%script bash

# This example has an error in VSCode; it runs best on Jupyter
cd /tmp

file="sample.md"
if [ -f "$file" ]; then
    rm $file
fi

# Set locale to C to avoid locale-related errors
export LC_ALL=C

# Create a markdown file using tee and here document (<<EOF)
tee -a $file >/dev/null <<EOF
# Show Generated Markdown
This introductory paragraph and this line and the title above are generated using tee with the standard input (<<) redirection operator.
- This bulleted element is still part of the tee body.
EOF

# Append additional lines to the markdown file using echo and redirection (>>)
echo "- This bulleted element and lines below are generated using echo with standard output (>>) redirection operator." >> $file
echo "- The list definition, as is, is using space to separate lines. Thus the use of commas and hyphens in output." >> $file

# Define an array of commands
commands=("ls" "cat" "tail" "pwd" "env" "grep" "awk" "sed" "curl" "wget")

# Loop through the commands array and append each command description to the markdown file
for cmd in ${commands[@]}; do
  description=$(man $cmd | col -b | awk '/^NAME/{getline; print}')
  echo "    - $description" >> $file
done

echo ""
echo "File listing and status"
ls -l $file # List file details
wc $file   # Show word count
mdless $file  # Render markdown from terminal (requires mdless installation)

#rm $file  # Clean up temporary file
```

    
    File listing and status
    -rw-r--r-- 1 kasm-user kasm-user 511 Dec 20 21:58 sample.md
     15  84 511 sample.md


    
    
    [0;37m[0;1;90;47mShow Generated Markdown [0;2;30;47m======================================================[0;37m
    
    [0;37mThis introductory paragraph and this line and the title above are generated using tee with the standard input (<<) redirection operator.[0;37m
    
     [0;1;91m* [0;97mThis bulleted element is still part of the tee body.[0;37m
     [0;1;91m* [0;97mThis bulleted element and lines below are generated using echo with standard output (>>) redirection operator.[0;37m
     [0;1;91m* [0;97mThe list definition, as is, is using space to separate lines. Thus the use of commas and hyphens in output.
     
      [0;97m[0;1;37;100m- [0;2;37;100m----------------------------------------------------------------------------[0;97m
     
      [0;97m[0;1;37;100m- [0;2;37;100m----------------------------------------------------------------------------[0;97m
     
      [0;97m[0;1;37;100m- [0;2;37;100m----------------------------------------------------------------------------[0;97m
     
      [0;97m[0;1;37;100m- [0;2;37;100m----------------------------------------------------------------------------[0;97m
     
      [0;97m[0;1;37;100m- [0;2;37;100m----------------------------------------------------------------------------[0;97m[0;37m
    
    [0m
