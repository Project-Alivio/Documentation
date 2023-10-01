# Setup 

## 1. **First Installations: Python, git, etc**

- macOS
    - `$ brew install python3 java git tree wget`
    - `$ brew install java`
- WSL/Linux
    - `$ sudo apt-get update`
    - `$ sudo apt-get install python3 python3-pip python3-venv python3-wheel python3-setuptools git tree default-jre`

## 2. **Web Dev Specific Installations**

- macOS
    - `$ brew install sqlite3 curl coreutils node`
- WSL/Linux
    - `$ sudo apt-get install sqlite3 curl nodejs npm`

## 3. **Clone the Repo from Github**

-  First, make sure you have Github Authentication set up. You can follow [this EECS 280 tutorial](https://eecs280staff.github.io/tutorials/setup_git.html#github-authentication) if you need help, and can choose to use with an SSH key or Personal Access Token (PAT).

> [!Warning]
> If you chose to use an SSH key to set up your Github Authentication, there is one more step you must take in order to be able to push and pull from Alivio's repositories. 

1. Navigate to Github on your browser, make sure that you are signed in, and go the the settings page. This can be done by clicking your profile icon in the top right corner of the page.
2. Now that you're in your settings, go to the SSH and GPG keys page. Find the key that you're using under the Authentication Keys section.
3. Then, for that particular key, click the Configure SSO button, and allow access to the Project Alivio organization. 

To clone the repo do one of the following: 

- *If you use SSH*: `$ git clone git@github.com:Project-Alivio/alivio-prioritization.git`
- *If you use PAT*: `$ git clone https://github.com/Project-Alivio/alivio-prioritization.git`

## 4. **Alivio Specific Installations**

- First, activate a virtual environment
    - `$ python3 -m venv env`
    - `$ source env/bin/activate`
- Next, install Python packages
    - `$ pip install -r requirements.txt` 
    - `$ pre-commit install`
- Then, install the front-end related packages
    - Run: `$ npm ci .`

## 5. Set Up .env (Environment) File (NEW FOR THIS YEAR, EVERYONE MUST DO IT)

- Navigate to the root of the alivio directory if you aren’t already there
- Run `$ touch .env`
- Open the .env file in an IDE like VSCode
- Add the following environment variables given by Swaraj to the file
> [!Warning]
> If you're on Windows/WSL, sometimes copying and pasting from some other file can add hidden characters or mess up the line endings in the .env file. After adding in the appropriate environment variables, run `$ dos2unix .env`. If you don't have dos2unix installed, run `$ sudo apt install dos2unix`.

## 6. **IDEs**

- VS Code
- Optional: DataGrip, this is really useful for running SQL queries
    - Sign up for a JetBrains Education license.
    - Follow the steps that JetBrains outlines as you apply (you’re going to have to activate it through your umich email)
    - When your account is all set up, download DataGrip for your appropriate OS
    - Launch DataGrip and create a new project with whatever name you want
    - On the left hand side of your screen, you’ll see the words “Database Explorer”, and there should be a “+” symbol button near it
    - Click the + symbol, then click “Data Sources”, and finally click PostgreSQL
    - A dialog box will pop up asking you to fill out the following fields:
        - **Name**: postgres@AWSRDS
        - **Host**: alivio-prioritization-db.c4tuxwdpwsfq.us-east-1.rds.amazonaws.com
        - **Port**: 5432
        - **Authentication**: User & Password
        - **User**: alivio
        - **Password**: ***ask Swaraj for this one***
        - **Database**: ebdb
        - **URL**: jdbc:postgresql://alivio-prioritization-db.c4tuxwdpwsfq.us-east-1.rds.amazonaws.com:5432/ebdb
- Hit “OK” in the dialog box and then you should see postgres@AWSRDS show up on the left hand side panel.
- To navigate to our DB tables, hit the drop down arrows on postgres@AWSRDS > ebdb > public > tables
- If you ever want to run a query, right click on postgres@AWSRDS, hit the “Jump to Query Console” option, and then “Open Default Console”
    - Type whatever query you want and hit the green run button (looks like a play button)


## 7. Run

1. Running the App on your Browser
    - cd into the project directory
    - Make sure virtual environment is activated
        - Run `$ source env/bin/activate`
    - Run `$ ./bin/alivio start`
    - Navigate to localhost:8000 in your browser
2. To create a new branch on git: `$ git checkout -b <name_of_branch>`


## 8. Debugging Tips

- For Python related debugging, using [pdb](https://docs.python.org/3/library/pdb.html) can be helpful.
    - As the linked documentation says, simply `import pdb` in the Python file you're debugging in and type in the line `pdb.set_trace()` above where you want to start debugging.
    - `pdb.set_trace()` now functions as a breakpoint.
    - Once the breakpoint is triggered, look at your terminal (from which the app was run) and you should see that you have entered the pdb console.
    - See the section on debugging commands in the linked documentation to understand how to use pdb.
- For JavaScript related debugging, using the [developer tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools) in your browser can be helpful.
    - Here, if you navigate to the Sources tab and open the alivio folder in the left side panel and go into the client directory, you should see all of the .jsx source files.
    - You can open up these files, set breakpoints wherever, and have them triggered as you interact with the UX.

## 9. Managing Node Versions

- Due to the fact that a significant chunk of Alivio's codebase was written 1.5 years ago, the latest, non-stable builds of Node aren't compatible with the Alivio application. However, if you're using a more recent version of node for another project on your computer, it's worth using `nvm` to manage changing versions of Node. (For example, taking EECS 485, and also doing Alivio). 
- This section will describe how to download nvm and switch between versions of Node. It is **OPTIONAL** and only needs to be done if you have issues with Node.  

### Downloading NVM

This section is written w/ the assumption that you're using WSL and Bash. If you're on MacOS you'll need to look up how to manage Node versions on MacOS. 

- To install NVM, and set the source lines, run the following commands: 

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
$ export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

- Then restart your terminal and run `$ nvm` to verify your installation. 

### Installing and managing Node.js versions 

- To install a Node version, run: 

```bash
$ nvm install --<version number>
```

- To see what versions of Node you have, run: 

```bash
$ nvm ls
```

- To switch node versions, run: 

```bash
$ nvm use <version number>
```

### Repairing a damaged Node installation

- Again, this section is written for a WSL / Linux User (I don't have a mac) :( 

```bash
$ sudo apt-get update 
$ sudo apt-get clean 
$ sudo apt-get autoremove 

# at this point try and reinstall node / npm again. If it doesn't work continue

$ sudo apt --fix-broken install 
$ sudo apt-get update && sudo apt-get upgrade
$ sudo dpkg --configure -a 
$ sudo apt-get install -f 

# at this point try and reinstall node / npm again. If it doesn't work continue 

# check for dependancies using
$ sudo dpkg --configure -a # Hopefully you get an empty output here. 
$ sudo apt-get remove nodejs 
$ sudo apt-get remove npm 

# cd to /etc/apt/sources.list.d and remove any node list you have 

$ cd ~/../../etc/apt/sources.list.d/ # an example path on a standard ubuntu set up 

# remove any node source list if there exists one. Then do: 

$ sudo apt-get update

# try reinstalling node js and npm again now. 
# if this doesn't work you're on your own :( 
```