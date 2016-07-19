# cf-towns-dotnet
#Introduction
This is a basic sample c# ASP.NET app demonstrating the functionaliy of retrieving the Cloud Foundry environment variable. If there is no MySQL service bound it will display a note stating this, If a MySQL service has been bound to the app it will populate the database with UK Town data and then display the results in a simple table.

#Getting Started
Once your Helion Stackato Windows DEA is available, you can simply clone this repo and then push the app as follows:

1) Check if your manifest file has the following contents:

applications:
- name: cf-towns
  mem: 256M
  buildpack: dotnet
  services:

2) Check the stacks and buildpacks confirgured in your environment by running:
stackato stacks
stackato buildpacks

3) Enter the appropriate stack and buildpack names in the push command e.g:
stackato push -n uktowns --stack <win2012r2> -buildpack <dotnet> 

You can then go to the app URL and you will see a message highlighting that you need to bind a MySQL Service, so go ahead and create and bind the service as follows

4) Check that you have the mysql service isntalled in your environemnt by running:
stackato services

and look for:
+------------+------+------------------------------------------+---------+------+--------+----------+---------+--------+------+
| Vendor     | Plan | Description                              | Details | Free | Public | Provider | Version | Broker | Orgs |
+------------+------+------------------------------------------+---------+------+--------+----------+---------+--------+------+
| mysql      | free | MySQL database service                   | N/A     | yes  | yes    | core     | 5.5     |        |      |

If not available, ask your stackato administrator to enable the mysql service on one of the stackato VM/nodes

5) stackato create-service-instance
cf create-service <service-name> <plan-name> <mysqlServiceName>
cf bind-service uktowns <mysqlServiceName>
cf restage uktowns

6) Alternatively, you can modify the manifest.yml file to include the dependent mysql service instance so that your app binds to the service at startup as follows:

applications:
- name: cf-towns
  mem: 256M
  buildpack: dotnet
  services: <mysqlServiceName>
    ${name}-db:
    type: mysql
Restart the app by running 
stackato restart uktowns

Then re-visit the URL to see the sample app showing a list of UK Towns
