In response to heightened security concerns, the xFusionCorp Industries security team has opted for custom Apache users for their web applications. Each user is tailored specifically for an application, enhancing security measures. Your task is to create a custom Apache user according to the outlined specifications:



a. Create a user named siva on App server 3 within the Stratos Datacenter.


b. Assign a unique UID 1013 and designate the home directory as /var/www/siva.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

## Solution

```
useradd -u 1013 -d /var/www/siva -m siva
```
