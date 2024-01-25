# 2023-SMODE-Open-Data-Workshop
November 9, 2023

## Recordings and Presentations

## Agenda

10:20am - Examples of use of S-MODE data access and use

## Prerequisites

To follow along hands-on during the Workshop, please do the following:

### Python environment

This project uses version 1.0.0 of the Coringa environment. For more details, visit the GitHub repository: https://github.com/iuryt/env_coringa.

### Earthdata Login account

Create an Earthdata Login account (if you don’t already have one) at [https://urs.earthdata.nasa.gov](https://urs.earthdata.nasa.gov)
Remember your username and password; you will need to download or access cloud data during the workshop and beyond.

S-MODE datasets can be found through Earthdata Search: [https://search.earthdata.nasa.gov/portal/podaac-cloud/search?fpj=S-MODE](https://search.earthdata.nasa.gov/portal/podaac-cloud/search?fpj=S-MODE)

#### Set up .netrc file for Earthdata login

The current tutorial available on this repository uses [earthaccess](https://github.com/nsidc/earthaccess) package to download data from PO.DAAC and will ask for your Earthdata username and password at every run, but you can also  setup a netrc file containing your NASA Earthdata Login credentials in order to execute the notebooks. A netrc file can be created manually within text editor and saved to your home directory. An example of the required content is below.

    machine urs.earthdata.nasa.gov
      login <USERNAME>
      password <PASSWORD>

<USERNAME> and <PASSWORD> would be replaced by your actual Earthdata Login username and password respectively.

**NOTE the .netrc file stores your password in plaintext - choose a unique password that is not used for other sensitive logins.**

You can also run the following script to automatically create your .netrc file if it does not yet exist:

    from netrc import netrc
    from subprocess import Popen
    from platform import system
    from getpass import getpass
    import os

    urs = 'urs.earthdata.nasa.gov'    # Earthdata URL endpoint for authentication
    prompts = ['Enter NASA Earthdata Login Username: ',
              'Enter NASA Earthdata Login Password: ']

    # Determine the OS (Windows machines usually use an '_netrc' file)
    netrc_name = "_netrc" if system()=="Windows" else ".netrc"

    # Determine if netrc file exists, and if so, if it includes NASA Earthdata Login Credentials
    try:
        netrcDir = os.path.expanduser(f"~/{netrc_name}")
        netrc(netrcDir).authenticators(urs)[0]

    # Below, create a netrc file and prompt user for NASA Earthdata Login Username and Password
    except FileNotFoundError:
        homeDir = os.path.expanduser("~")
        Popen('touch {0}{2} | echo machine {1} >> {0}{2}'.format(homeDir + os.sep, urs, netrc_name), shell=True)
        Popen('echo login {} >> {}{}'.format(getpass(prompt=prompts[0]), homeDir + os.sep, netrc_name), shell=True)
        Popen('echo \'password {} \'>> {}{}'.format(getpass(prompt=prompts[1]), homeDir + os.sep, netrc_name), shell=True)
        # Set restrictive permissions
        Popen('chmod 0600 {0}{1}'.format(homeDir + os.sep, netrc_name), shell=True)

        # Determine OS and edit netrc file if it exists but is not set up for NASA Earthdata Login
    except TypeError:
        homeDir = os.path.expanduser("~")
        Popen('echo machine {1} >> {0}{2}'.format(homeDir + os.sep, urs, netrc_name), shell=True)
        Popen('echo login {} >> {}{}'.format(getpass(prompt=prompts[0]), homeDir + os.sep, netrc_name), shell=True)
        Popen('echo \'password {} \'>> {}{}'.format(getpass(prompt=prompts[1]), homeDir + os.sep, netrc_name), shell=True)


## Tutorials and Data Access

S-MODE data can be found through Earthdata Search here: [https://search.earthdata.nasa.gov/portal/podaac-cloud/search?fpj=S-MODE](https://search.earthdata.nasa.gov/portal/podaac-cloud/search?fpj=S-MODE)


## Science Case Study 
 
Under `notebooks` directory:

- DopplerScatt_SurfaceDrifters.ipynb : download, process, map and visualize DopplerScatt and surface drifters data.

