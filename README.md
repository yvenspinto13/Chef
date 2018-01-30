*****************************************************************************************************
					CHEF README
*****************************************************************************************************

This is a cookbook written to install and configure a sample application using tomcat and mysql on a linux chef node. When in production the node will keep replacing the war file in tomcat with the most recent one given to the node from jenkins.


*****************************************************************************************************

To run the demo(Quickstart):
	1) Bootstrap your node using knife.
	2) Edit run list, initially configure your environment as "development", this will install 	   	   and configure tomcat 8 and mysql. A database dump file is provided which will dump your 	   database required for the sample application.
	3) Switching your node environment to "production" will run the recipe for replacing your war 	   file in tomcat.


*****************************************************************************************************


Dependencies:
	1) tomcat cookbook
	2) java cookbook



*****************************************************************************************************

Notes:
	1) Targeted node machine is Ubuntu Linux.
	2) Apache Tomact 8 is installed which needs java 8.
	3) Mysql will be installed via apt-package.
	4) Mysql dump is done via the bash script.
	5) Make sure you fullfill the dependencies of all cookbooks when uploading to your chef 	           server.


*****************************************************************************************************


The main cookbook written is the javawebapp cookbook

  Recipes:
	    1) default.rb
		This is the initial recipe made, it was used during the testing phase of chef.

	    2) package_database.rb
		This recipe installs mysql from apt and sets the basic configuration, it sets				password and user to "root". The recipe consumes a "my.cnf" file which configuers			mysql to talk over a socket and with permissions. After installation of mysqlthe 			recipe uses a .sql file to dump the database needed by the Sample Application to run.
		The files are available in the cookbook/files directory. You can change it as you 			wish however make sure the naming conventions remain.
	    
	    3) tomcat_server.rb
		This recipe installs apache tomcat 8 and uses a sample war file from your cookbook 			files directory. It makes use of the java cookbook from chef supermarket to 				make sure that the required version of java is installed on your node. The recipe 			makes use of the tomcat cookbook from chef supermarket that allows you to install the 		appropriate tomcat instance and allows you to perform various actions on your tomcat 			service.The tomcat instace is installed and belongs to a "javawebapp_group".

	    4) replace_war.rb
		This recipe stops the tomcat instance that is running, it then clears the webapp 			folder of your tomcat instance. After clearing it, the recipe copies the war file 			from the /warfiles directory to the tomcat webapps folder. It then restarts tomcat.

	    5) database.rb
		This recipe was written as an attempt to install mysql via the mysql cookbook				available on the chef supermarket. However there were issues faced and connecting to 			the mysql sequence was a problem. We then moved to the package_databse.rb recipe.

	    6) handle_tomcat_service.rb
		This recipe was written to perform simple restart, start and stop of the installed 			tomcat instance, which can be used during the testing or debugging phase.
		


*****************************************************************************************************


Environments : production, development

Roles : web_server


*****************************************************************************************************
