# PE Status Check Dashboard

This is a POC of a dashboard created for the [PE_Status_Check module](https://forge.puppet.com/modules/puppetlabs/pe_status_check#fact-pe_status_check). It will provides an easy view of all the infrastrcture nodes on the system and their status which is pulled from the module.

The application will store a `json` file under `dashboard_pe_status/app/data/pe_statu.json` which will update once the website is refreshed and 10min has passed from the last run. Please note that the page will take around 10 seconds to load as the plan needs to run on the primary node and return the data from all the infrastructure nodes.

## Setup

You will need:
* Python 3
* PIP (usually bundled with Python)
* [PE_Status_Check module](https://forge.puppet.com/modules/puppetlabs/pe_status_check) installed on your infrastructure

Clone the repo to your directory and install the dependencies from `requirements.txt`:

```
git clone https://github.com/BartoszBlizniak/dashboard_pe_status.git
cd dashboard_pe_status
pip3 install -r requirements.txt
```
If your Python version doesn't meet the requirements, the two main dependencies are:

```
pip install Flask
pip install requests
```

## Usage

The run command takes 2 positional parameters:
* Certname of the primary server
* RBAC access token

The example below utilises `puppet` commands to pull them automatically, please note that this will only work from the Primary server node, if you want to run the dashboard on a different host, you will have to replace the commands with appropriate values.

Check if the application is working:

```
cd dashboard_pe_status/app
python3 main.py $(puppet config print certname) $(puppet-access show)
```
If the dashboard is not hosted on the primary server:

```
python3 main.py FQDN_OF_PRIMARY RBAC_TOKEN
```

The dashboard should then be accessible from: `http://IP_OF_DASHBOARD_HOST:5000`

To run the dashboard in the background, simply add `nohup` at the beginning of the `python3` command and close the shell (Don't `CTRL+C` as it will kill the process).

