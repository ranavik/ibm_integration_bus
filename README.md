`ibm_integration_bus` Cookbook
==========================
[IBM® Integration Bus](http://www-03.ibm.com/software/products/us/en/integration-bus/), formerly known as WebSphere® Message Broker, is an enterprise service bus (ESB) providing connectivity and universal data transformation for service-oriented architecture (SOA) and non-SOA environments. Businesses of any size can eliminate point-to-point connections and batch processing regardless of platform, protocol, or data format.


You can find more information about using IBM Integration Bus from the [IBM Integration Bus Version 9.0 Information Center](http://pic.dhe.ibm.com/infocenter/wmbhelp/v9r0m0/index.jsp).


Use this cookbook to install and configure all the components that you need to start developing applications with [IBM Integration Bus](http://www-03.ibm.com/software/products/us/en/integration-bus/), including:


* IBM Integration Bus runtime component
* IBM Integration Bus Toolkit
* IBM Integration Bus Explorer  
* All product prerequisites, including WebSphere MQ Version 7.5.0.1  

The cookbook also creates a user account to manage the components, and 
creates and configures all the components that are required for a running  Integration Bus instance.

For answers to questions about this cookbook see the provided [FAQ](./FAQ.md).


Requirements
------------
To use this cookbook, you must download the Developer Edition of IBM Integration Bus and store it on an FTP or HTTP server that is accessible by the computer running the chef client.

IBM Integration Bus for Developers is a full-function version of the product, which you can use for 
evaluative purposes. You can [download](https://www14.software.ibm.com/webapp/iwm/web/signup.do?source=swg-wmbfd&S_TACT=109KA7GW&S_CMP=web_opp_ibm_ws_appint_integrationbus&lang=en_US&S_PKG=dk) this version at no charge and you are free to use it for as long 
as you require, within the terms of the license. 

 
## Chef

* Chef: 0.10.10+

## Cookbooks
No other cookbooks are required by this cookbook. The IBM WebSphere MQ  product is installed as part of both the default and runtime recipes.


## Platforms

The cookbook currently supports:

* Ubuntu 12.04 LTS x86-64

It is recommended that this cookbook is run only on systems that do  not have any other versions of IBM WebSphere MQ or IBM Integration Bus installed.

Attributes
----------
#### ibmintegrationbus::default
<table>
  <tr>
    <th>Key</th>
    <th>Type</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td><tt>['ibm_integration_bus']['package_site_url']</tt></td>
    <td>String</td>
    <td>URL of an FTP server, HTTP server, or file system that contains the install package.</td>
    <td><tt>nil</tt></td>
  </tr>
  <tr>
    <td><tt>['ibm_integration_bus']['package_name']</tt></td>
    <td>String</td>
    <td>Name of the install package on the URL site.</td>
    <td><tt>9.0.0-IIB-LINUXX64-DEVELOPER.tar.gz</tt></td>
  </tr>
  <tr>
    <td><tt>['ibm_integration_bus']['account_username']</tt></td>
    <td>String</td>
    <td>Name of the user account to set up for use with the installed components. If the account does not exist,   it will be created.</td>
    <td><tt>iibuser</tt></td>
  </tr>
  <tr>
    <td><tt>['ibm_integration_bus']['account_password']</tt></td>
    <td>String</td>
    <td>A shallow hash of the password to use for account_username. Use a command like openssl passwd -1 "actual_password" to generate the hashed password. If this attribute is not set, the user account is created without a password.</td>
    <td><tt>nil</tt></td>
  </tr>
</table>

Usage
-----
Include `ibm_integration_bus` in your node's `run_list` to install and set up all IBM Integration Bus components. The `package_site_url` attribute must be set to the location of the downloaded Developer Edition installation package. [Download](https://www14.software.ibm.com/webapp/iwm/web/signup.do?source=swg-wmbfd&S_TACT=109KA7GW&S_CMP=web_opp_ibm_ws_appint_integrationbus&lang=en_US&S_PKG=dk) this package and put it onto an FTP or HTTP site that is accessible by the chef client running the recipes. The `package_site_url` points to the directory containing the install package and the full URL is generated by appending `package_site_url` and `package_name` together. For example: if `package_site_url` is `ftp://company_ftp_site.com/iib_packages` and  `package_name` is left to the default value of `9.0.0-IIB-LINUXX64-DEVELOPER.tar.gz` then the URL used is `ftp://company_ftp_site.com/iib_packages/9.0.0-IIB-LINUXX64-DEVELOPER.tar.gz`.


To install the full IBM Integration Bus:

```json
{
  "name": "Full_install",
  "description": "",
  "json_class": "Chef::Role",
  "default_attributes": {
  },
  "override_attributes": {
    "ibm_integration_bus": {
      "package_site_url": "ftp://company_ftp_site.com/iib_packages"
    }
  },
  "chef_type": "role",
  "run_list": [
    "recipe[ibm_integration_bus]"
  ],
  "env_run_lists": {
  }
}
```


To install all components except the toolkit component,   select the runtime recipe:
 
```json
{
  "name": "No_toolkit",
  "description": "",
  "json_class": "Chef::Role",
  "default_attributes": {
  },
  "override_attributes": {
    "ibm_integration_bus": {
      "package_site_url": "ftp://company_ftp_site.com/iib_packages"
    }
  },
  "chef_type": "role",
  "run_list": [
    "recipe[ibm_integration_bus::runtime]"
  ],
  "env_run_lists": {
  }
}
```

To set up a password for the created user account:
```json
{
  "name": "Account_Password",
  "description": "",
  "json_class": "Chef::Role",
  "default_attributes": {
  },
  "override_attributes": {
    "ibm_integration_bus": {
      "package_site_url": "ftp://company_ftp_site.com/iib_packages",
      "account_username": "aUser",
      "account_password": "$1$GjlS21O/$cCVKEGE/qg4jjt.sJuwVK0"
    }
  },
  "chef_type": "role",
  "run_list": [
    "recipe[ibm_integration_bus]"
  ],
  "env_run_lists": {
  }
}
```



License and Authors
-------------------
Copyright 2013 IBM Corp. under the [Eclipse Public license](http://www.eclipse.org/legal/epl-v10.html).

* Author:: John Reeve <jreeve@uk.ibm.com>
* Author:: Simon Holdsworth <Simon_Holdsworth@uk.ibm.com>
* Author:: Imran Shakir <SHAKIMRA@uk.ibm.com>
* Author:: Stephanie Strugnell <stephanie_strugnell@uk.ibm.com>

