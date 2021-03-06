# Packtpub checker

[![Build Status](https://travis-ci.org/mjarosie/packt_checker.svg?branch=master)](https://travis-ci.org/mjarosie/packt_checker)

I proudly present packtpub_checker script, which can be used to set as a windows scheduled task
to scrap packtpub.com free learning offers page in order to send its content about free book by email.

Continuous integration is provided by running tests on travis-ci platform. Configuration of this process is placed in .travis.yaml file.

## Usage

Application usage:

```python check_packt.py [-u url_to_packtpub_free_offer] [-s sender] [-r recipient_1] [-r recipient_2] ...```

If any of those optional arguments is missing, script tries to derive any missing parameters from config.py file.

## Running tests

In order to run tests run ```nosetests``` in the root directory of the project.
To run the tests with code coverage, run ```nosetests --with-coverage --cover-package=src --cover-inclusive```

## Schedule as a task

First, you need to set up a virtual environment: ```python3 -m venv /path/to/new/virtual/environment```

Afterwards ```git clone``` this project into the root directory of virtualenv that you just created.

Next, activate the virtual environment: ```source /path/to/new/virtual/environment/bin/activate```

(On windows it will be slightly different: ```/path/to/new/virtual/environment/Scripts/activate```)

Then change directory into the project directory and install the needed packages listed in requirements.txt using pip: ```python -m pip install -r requirements.txt```

### Unix

On *nix systems you need to add an entry to crontab (cron table) list.
To do this, use the following command:

```crontab -e```

and then write a following entry into the file:

```0 12 * * * cd /virtual/env/root/directory && source bin/activate && python packt_checker/src/check_packt.py >> /path/to/log.txt 2>>/path/to/error.txt```

This entry will tell cron to do the following commands:
- At every 0-th minute of 12-th hour of every day of every month, every weekday, do the following task:
- cd (change directory) to /virtual/env/root/directory (this is where your virtualenv is).
- if the previous command succeded:
- run bin/python3 with the packt_checker/src/check_packt.py as an argument. This command will just run the main script with python3.
- save the output of script to the log.txt file.

### Windows

On Windows in order to schedule the task to run daily at 12:00, you need to run the following command in cmd with administrator privileges.

```schtasks /create /sc DAILY /tn packtpub_checker /tr "dir:\path\to\python\python.exe dir:\path\to\app\check_packt.py [script_parameters]" /st 12:00```

In order to check the usage of schtasks, go [there](https://technet.microsoft.com/en-us/library/cc725744.aspx)