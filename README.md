# Discord-Bot-for-Wazuh

Integrating Wazuh with Discord.

* My goal is to utilize Wazuh and Suricata in tandem to monitor my home network. Additionally, I would like a user-friendly method of viewing the alerts produced by Wazuh.
Creating a Discord Server

* Open Discord and log in to your account.
* On the left-hand side of the Discord interface, click on the plus icon next to the server list.
* Click on "Create a Server" and choose a name for your server.
* Select a region closest to you to ensure good server performance.
* Choose an icon for your server, such as an image or logo.
* Once you have completed these steps, click on the "Create" button to create your server.
* You have now successfully created a server in Discord.

Creating a webhook
* Open Discord and go to the server where you want to create the webhook.
* Click on the server settings by clicking on the server name and then clicking on "Server * Settings" at the bottom of the drop-down menu.
* Click on "Integrations" from the left-hand menu.
* Click on the "Create Webhook" button.
* Enter a name for your webhook and select the channel where you want the webhook messages to be sent.
* Choose an icon for your webhook, such as an image or logo.
* Once you have completed these steps, click on the "Create" button to create your webhook.
* You have now successfully created a webhook in same server you had created.

# Integration part
* move back to Wazuh server and change directory 
* cd /var/ossec/integrations
* list all file in the directory.
* ll (see the list)
* copy the slack files with following 2 command.
    1. cp slack custom-discord
    2. cp slack.py custom-discord.py 
* Now open the cusotom-discord.py and paste the custom-discord.py file from my repo.
* open the main ossec.conf file.
* nano /var/ossec/etc/ossec.conf

        <integration>
                <name>custom-discord</name>
                <hook_url>https://discord.com/api/webhooks/hook</hook_url>
                <level>7</level>
                <alert_format>json</alert_format>
        </integration>
* now change the permission and owner of the custom-discord* files.
    1. chown root:wazuh cusotom-discord*.
    2. chmod 750 cusotom-discord*.

# Creating custom rule 
* creat a new file.
* touch /var/log/test.log
* now add the rule to fetch the log of test.log file.
* move to wazzuh manager and open the file form following location.
    1) Wazuh drop down. -> Managment. -> Groups. -> Click on the pencil icon shown in default group.
    2)select the agent.conf and add the following rule set.

        <localfile>
            <location>/var/log/test.log</location>
            <log_format>syslog</log_format>
        </localfile>
* Open the local rule set file.
* nano /var/ossec/etc/rules/local_rule.xml
* add following command.

        <rule id="119999" level="12">
            <regex>^test$</regex>
            <description>Test rule to configure integration</description>
        </rule>
* Run the binary and type the word test, you should see your rule getting triggered.
* /var/ossec/bin/wazuh-logtest
* type test in the wazuh-server(where you are running the command.)
* Restart the manager and trigger the alert, you should receive the alert on your Discord channel.
* /var/ossec/bin/wazuh-control restart.
* echo -e "test" /var/log/test.log

## Acknowledgements

 - [Eugenio Chaves](https://eugenio-chaves.github.io/blog/2022/creating-a-custom-wazuh-integration/)
 - [Discord Webhook](https://www.make.com/en/blog/guide-to-discord-webhooks)
 

## Authors

- [@harsh0071999](https://github.com/Harsh0071999)


## Feedback

If you have any feedback, please reach out to us at harshmgandhi20@gnu.ac.in

