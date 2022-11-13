# Elevated Custom Command

#### Introduction

There are situations where you'd want to launch a CustomCommand with elevated privileges, for instance if the application requires it. HASS.Agent can't natively do that (as a security measure), but you can use Windows' [Scheduled Tasks](https://en.wikipedia.org/wiki/Windows_Task_Scheduler) instead.

In short, scheduled tasks are a way to have Windows perform certain tasks on predetermined times or events. An example could be *open notepad when I log into Windows*  or *run this script at 5AM every first of the month*. 

#### Creating the Scheduled Task

You can open the scheduler by searching for `Task Scheduler` from the task menu, or by executing `taskschd.msc`:

![image](https://user-images.githubusercontent.com/81011038/201527584-b827f0b9-9ac4-410d-8c0a-ddf0ac4f7582.png)

Click on `Task Scheduler Library` on the left to view a list of your current tasks (there are also subfolders, but the root folder usually contains the interesting ones). To create a new task, click on `Create Task` on the right pane.

Give it an appropriate name, describing what it does. Enable `Run with highest privileges` to make sure it can run elevated:

![image](https://user-images.githubusercontent.com/81011038/201527732-38ced999-39cd-446c-a47f-c54dbf83d77a.png)

> Tip: if it's a script (something that closes itself when done), you can check `Hidden` to have it run in the background, no interface.

The `Trigger` tab allows you to configure moments or events on when to run this task, but we'll do that through HASS.Agent, so you can skip that part. Click on the `Actions` tab -> `New` and enter the application you want to run:

![image](https://user-images.githubusercontent.com/81011038/201527916-df1aa9ba-b2a4-429f-9937-472d74f53dab.png)

> Tip: it's always best to provide the `Start in` path as well.

WHen you enter it like this, your scheduled task will remaing in `Running` state until you close the application. You'll be able to start it once. If you just want to start your application, and then stop the scheduled task, you can use `cmd` as the program, and `/C "C:\Windows\notepad.exe"` as the arguments. Just replace `C:\Windows.notepad` with your app, leave the `/C `:

![image](https://user-images.githubusercontent.com/81011038/201528036-55a8f7b2-1b9a-4f55-a6e6-39f56f32fb7c.png)

> Note: if you're running a script, something without an interface, you don't need the `cmd /C` trick.

Click `OK` to store your action.

Go to the `Conditions` tab, and decide whether you want to be able to launch the task when on battery (only relevant for a laptop or if you use an UPS):

![image](https://user-images.githubusercontent.com/81011038/201528087-3f16424a-91c9-46fe-b93d-e751e52b1b89.png)

Go to the `Settings` tab, and configure as follows (of course, feel free to change as you see fit):

![image](https://user-images.githubusercontent.com/81011038/201528121-75ed6c6c-a2ed-49bf-b9c5-ac8f25a0d60b.png)

Click `OK` to store your scheduled task. You might be asked for your credentials, since we asked it to be elevated.

#### Creating the Custom Command

Close the Task Scheduler and open HASS.Agent. In the main window, click `Commands` -> `Add New`. Select the `Custom` type in the list. 

In the command field, enter the following with your task's name, in my case: `schtasks /Run /TN "HASS.Agent Notepad Elevated"`

Replace `HASS.Agent Notepad Elevated` with the name you gave your task. Should look like this:

![image](https://user-images.githubusercontent.com/81011038/201528548-80324d58-a7c2-4818-a605-c34abbe27a38.png)

Click `Store Command` -> `Store and Activate Commands`. 

#### Testing

Go to your device's page in Home Assistant, and click on your new command. The application you chose should now launch, elevated!
