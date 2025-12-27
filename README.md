# Active Directory Lab 3: Security Groups
In this lab, I demonstrate how Active Directory security groups are used to control access to resources and scope Group Policy Objects (GPOs). The lab focuses on group-based access control rather than assigning permissions directly to users, which is more useful than applying permissions user by user.
* Previous labs: https://github.com/johnweghorst/ad-lab, https://github.com/johnweghorst/ad-lab-2-gpo
# Skills Demonstrated
* Active Directory security group creation and management
* Group-based access control
* NTFS and share permission configuration
# Software and Tools Used
* Windows Server 2019
* Windows 10
* Active Directory Users and Computers 
* Group Policy Management 

# Objective
* Use security groups to control access to a shared folder instead of assigning permissions directly to individual users.
* On the domain controller, we’ll set up a security group. Open up Active Directory Users and Computers, go to the _USERS OU created in the first lab, and create two new Groups, one called Test-Share-Read, and one called Test-Share-Modify.

<img width="947" height="695" alt="image" src="https://github.com/user-attachments/assets/e727f199-924c-4c4a-9ccc-3bed516ab1a9" />

* Choose ‘Global’ and ‘Security’.
* I am going to expand on the options here a bit for clarity but they are out of the scope for this lab:
    * Domain local groups grant permissions within a single domain (for example, a domain local group can be used to assign a permission or resource in that exists in mydomain.com only)
    * Global group scopes are used to group users from the same domain and can be granted access to resources in other trusted domains within the same Active Directory forest. They are commonly used to manage user access across trusted domains. Example: A Global group created in mydomain.com can be granted access to specific resources in tw.mydomain.com.
    * Universal groups are used to grant permissions across multiple domains within the same forest. These groups can be assigned permissions to resources in any domain in the forest and are especially useful when users from several domains need the same access. Universal groups are ideal for larger environments with multiple domains. Example: A Universal group such as Global-Admins in mydomain.com can be granted permissions to resources in tw.mydomain.com, uk.mydomain.com, and other domains in the same forest, making it more suitable for managing access across the entire forest.
    * A security group is used to assign permissions to resources and can also be used for Group Policy security filtering.
    * A distribution group is used to create email distribution lists.

* We will be choosing a Global group here, even though we are working with a single domain because they are the recommended scope for grouping users. This design will allow for easy scalability if additional domains are added in the future. Create a group called ‘Test-Share-Read’, make it global, and security type. We’ll be choosing security because we want to grant permission to a resource. Create a 2nd group with the same options and name it ‘Test-Share-Modify’.

<img width="975" height="739" alt="image" src="https://github.com/user-attachments/assets/7398a2de-3ba6-4c11-8782-9c3d66f9ce58" />
<img width="677" height="588" alt="image" src="https://github.com/user-attachments/assets/7c0413b9-82fd-4a3b-b6f1-996fd8a50765" />

* Add a user to each group. We can do this by right-clicking a user and choosing the ‘Member Of’ tab, or by right-clicking the security group and adding them that way. We’ll do one of each.

<img width="975" height="847" alt="image" src="https://github.com/user-attachments/assets/0ed06bdc-537c-43ab-a052-453a3fd9d483" />
<img width="645" height="842" alt="image" src="https://github.com/user-attachments/assets/cdb169ee-e824-47b1-93f2-c40c8abf72c5" />
<img width="839" height="719" alt="image" src="https://github.com/user-attachments/assets/0f8394ac-a1c8-4151-a7b3-0b6f76b5a971" />
<img width="628" height="709" alt="image" src="https://github.com/user-attachments/assets/778765de-39cf-47cb-8ca4-76389c5d8458" />

* Next, we’ll create a folder for sharing on the domain controller.
* We’ll called the folder ‘Test-Share’ and place it in the Shares folder on the domain controller’s C: drive. We’ll also create a text file so we can see the permissions in action. 

<img width="775" height="222" alt="image" src="https://github.com/user-attachments/assets/468c48ed-5741-497c-96a8-cf48720895a3" />
<img width="975" height="953" alt="image" src="https://github.com/user-attachments/assets/4effb87c-52fe-47c6-baf4-329c472bd6ea" />

* Right-click the Test-Share folder and select Properties, then Sharing, and then Advanced Sharing. On the ‘Network’ tab, add the groups we created. ‘Test-Share-Read’ with Read, and ‘Test-Share-Modify’ with modify rights. Share the folder.

<img width="945" height="694" alt="image" src="https://github.com/user-attachments/assets/e53b0f35-a513-4ca2-821e-2a9c6fd06ccf" />

* Next, we’ll head to the Security tab. As you can see, our groups have been automatically added.

<img width="570" height="748" alt="image" src="https://github.com/user-attachments/assets/c1c19f3d-64c4-437c-af7e-5158fd356de4" />

* These add a little bit more options to what can be seen on the Network access box. You may choose to add those here instead if you need more explicit options.
* Now, we’ll be able to test our groups. I’ll log into the afiles user, who is in the ‘Read’ group and has that option only. 
* **Note here, I could not get the shared folder to come up in ‘Network’ on the client PC. I tried several things, as far as I can tell it should have worked (Port 445 in bound was correctly configured on the DC firewall). However, accessing the share by pasting the UNC path did work. Windows+R, \\\\WIN-7AS15TP70BQ\\Test-Share. Troubleshooting this is out of the scope of this lab and may be continued at some point here. This lab is meant to demonstrate simple group-based permissions. Access the folder by copy and pasting the path from the \\\\"DC name"\\"share name"**

* When trying to save our edit on the afiles account, this error pops up.
<img width="975" height="532" alt="image" src="https://github.com/user-attachments/assets/aad5d528-9fe9-4a99-bcc3-88dcd1e3d6f5" />

*We log into the ablackwater account and are able to successfully read and modify the text file.

<img width="975" height="607" alt="image" src="https://github.com/user-attachments/assets/396eb7cb-499b-4761-8c6d-7d0d7b6e0488" />

* We can also create a file with this account.

<img width="975" height="622" alt="image" src="https://github.com/user-attachments/assets/73d90c9c-3f93-4570-b3ee-425b618c56cf" />

* Back onto afiles account, we can see the new file as well as the Test text file file has updated our save.

<img width="748" height="316" alt="image" src="https://github.com/user-attachments/assets/09771dd7-03cf-405c-aac4-9790f19919cb" />
<img width="875" height="395" alt="image" src="https://github.com/user-attachments/assets/bd9b962a-90f0-4d21-aad2-d3fb4092400a" />

* We know ablackwater have saved their additions because because as an asterisk * appears when editing an unsaved text file, as seen on afiles account.

<img width="898" height="381" alt="image" src="https://github.com/user-attachments/assets/02feeb0f-dab4-4544-b85d-5e6f17d51af7" />

* In this lab, the goal for me was to become more familiar with Active Directory groups. I learned how basic security group functions provide access to shared resources based on group membership, and how group membership determines permissions. This approach makes managing access more efficient and scalable than assigning permissions directly to individual users.
* Thanks for following along this lab! 
