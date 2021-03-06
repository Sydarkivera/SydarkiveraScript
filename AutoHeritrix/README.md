<!--- State the text needed in the fields marked with [explanatory on what] if not needed remove the text. 
Feel free to use more mark down for formatting the text-->
# AutoHeritrix

## Explanation of the script
AutoHeritrix is a script intended to automate the creation, building and launching of jobs in Heritrix 3.
The script uses Python Requests library to communicate with Heritrix REST API, launching jobs based on a list of URLs.

Please note that the script is intended to be run specifically in Sydarkiveras production environment. The script will not work properly without editing the code to fit other hardware- and software setups.

## Dependencies
The script is designed to run in a Linux environment and tested in CentOS 7 (https://www.centos.org/).

For running this script there are dependencies to the following software libraires:

-	Python 3 (https://www.python.org/)
-	Requests (https://requests.readthedocs.io/)
- Heritrix 3 (https://github.com/internetarchive/heritrix3)

A txt-file containing a list of URLs to be crawled is required as well as an XML-template for Heritrix configuration.
Please refer to the Heritrix 3 documentation for information on how to install and configure Heritrix.
 
## How to run the script
1.  Place the script, the template and the URL-list in a folder, for instance under /home.
2.  Make sure Heritrix 3 is running and properly configured to run jobs.
3.  Edit the script to specify the path of the template and URL-list (if seperate) and the URL and authentication details used for your Heritrix 3.
4.  Edit the template ```crawler-beans.xml```with your crawl options.
4.  Run the script in your Python 3 environment: ```$ python3 AutoHeritrix.py```

### Steps of the script
1.  Specify an ID for your crawls. The ID will be used to name the Heritrix jobs. For example, the current date.
2.  The program will list the URLs found in the URL-list combined with the ID you specified, after which you will be asked whether to create the jobs in Heritrix. Reply Y or N and press Return.
3.  Wait for the job creation to finish. If everything is set up properly you should now be able to see the jobs listed in the Heritrix web interface. Make sure to refresh the page if you don't. 
4.  The program will ask whether you want to build and run the jobs. Reply Y or N and press Return.
5.  Wait for the script to finish building and running the jobs. Make sure to check the Heritrix web interface to confirm whether the jobs have been started.
6. You're done!

## Monthly
Sydarkivera uses a modified version of the script for more frequent automated crawls of select webpages.
The monthly variant of AutoHeritrix is meant to be scheduled using job scheduler software (like cron) and will not ask for any user input when run.
Instead, all variables needed for the script to function are defined inside the script itself, requiring some editing before it will run.

For example, you can schedule it using crontab: ```* * * *    /usr/bin/python3    /path/to/script/AutoHeritrixMonthly.py >> /path/to/logs/autoHeritrixLog.txt```

The script uses the same template and URL-list structure to input and output data. If you are running both the standard version and the monthly version, it is recommended to keep them seperated in different folders with their own templates and URL-lists.

Once jobs have been run, the script will let them run for 48 hours before performing a teardown on them to save resources and prepare them for the next build and run.
Teardowns can also be performed manually by running the script ```teardown.py```.

## Description by:
Magnus "PreAmbience" Heimonen, 2020-06-24
