The DevOps team of xFusionCorp Industries is planning to create a number of Jenkins jobs for different tasks. So to easily manage the jobs within Jenkins UI they decided to create different views for all Jenkins jobs based on usage/nature of these jobs, - for example devops-crons view for all cron jobs. Based on the requirements shared below please perform the below mentioned task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


1. Create a Jenkins job named devops-test-job.

2. Configure this job to run a simple bash command i.e echo "hello world!!".

3. Create a view named devops-crons (it must be a global view of type List View) and make sure devops-test-job and devops-cron-job (which is already present on Jenkins) jobs are listed under this new view.

4. Schedule this newly created job to build periodically at every minute i.e * * * * * (please make sure to use the cron expression exactly same how it is mentioned here)

5. Make sure the job builds successfully.


Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution

https://www.nbtechsupport.co.in/2021/07/jenkins-create-views.html

- Just check the my-views

<img width="1919" height="690" alt="image" src="https://github.com/user-attachments/assets/10fd4be5-1660-468a-bdbb-1d3af1aa0278" />


- Create a Jenkins job named 'devops-test-job' and select 'free-style'

<img width="1645" height="843" alt="image" src="https://github.com/user-attachments/assets/95230a4f-7cd2-4b65-a573-c1d9ac01c5a8" />

- Add build step, Configure this job to run a simple bash command i.e echo "hello world!!".

<img width="1912" height="781" alt="image" src="https://github.com/user-attachments/assets/a5b46ffe-7cbd-4179-b6de-7917afdb9ad6" />

- Schedule this newly created job to build periodically at every minute i.e * * * * * (please make sure to use the cron expression exactly same how it is mentioned here)

<img width="1876" height="700" alt="image" src="https://github.com/user-attachments/assets/adda4923-4280-493e-983f-148a94538161" />

- save now and run the job

<img width="1919" height="642" alt="image" src="https://github.com/user-attachments/assets/779ea1be-7630-4975-b8c9-a29112346aca" />

- click new view

<img width="1919" height="476" alt="image" src="https://github.com/user-attachments/assets/cb09b946-ac2f-4573-85ac-2d07f5e5f62e" />

- Create a view named devops-crons (it must be a global view of type List View) and make sure devops-test-job and devops-cron-job (which is already present on Jenkins) jobs are listed under this new view.

<img width="1908" height="643" alt="image" src="https://github.com/user-attachments/assets/e1aec13e-23a9-4a3e-83b1-5963c93ab642" />

- now click save
<img width="1683" height="837" alt="image" src="https://github.com/user-attachments/assets/09278683-a2b3-4d27-9a2c-5850cf1c6e87" />

- now we can see new view got created
<img width="1919" height="616" alt="image" src="https://github.com/user-attachments/assets/6f0823a3-a78a-4331-882a-b3d5a3c5b05c" />







