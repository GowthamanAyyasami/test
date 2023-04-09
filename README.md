# Steps to migrate to EKS environment

This Article explains the steps for migrating the test project to EKS environment.

* [Step 1: Import the projects](#import-projects)

* [Step 2: Open the pom.xml of the RapidTestBoot](#edit-pom.xml-rapidtestboot)

* [Step 3: Change RTF Version in banner.txt](#change-rtf-version-banner)

* [Step 4: Open the pom.xml of the Test Project](#edit-pom.xml-testproject)

* [Step 5: Edit the script class](#edit-test-script-class)

* [Step 6: Add the properties](#add-properties-elementrepo)

* [Step 7: Update config.yml](#update-config.yml)

* [Step 8: Update the docker file](#update-docker-file)

* [Step 9: Update the rapidTestConfig.properties](#update-rapidTestConfig-properties)

* [Step 10: Create a job.yml](#create-job.yml)

## Step 1: Import the projects <a name="import-projects"></a>
Import the test project and the RapidTestBoot Project to be migrated

## Step 2: Open the pom.xml of the RapidTestBoot <a name="edit-pom.xml-rapidtestboot"></a>

Open the pom.xml of the RapidTestBoot project and change the RTF
version to the latest.

Currently it is 2.12. Also add the below dependency in the dependencies
tag

    <!-- https://mvnrepository.com/artifact/com.amazonaws/aws-java-sdk-sts -->
		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-java-sdk-sts</artifactId>
			<version>1.12.115</version>
		</dependency>

## Step 3: Change RTF Version in banner.txt <a name="change-rtf-version-banner"></a>
If there is any change in the RTF version, then change the
version of RTF in the banner.txt in src/main/resources

## Step 4: Open the pom.xml of the Test Project <a name="edit-pom.xml-testproject"></a>
Open the pom.xml of the test project and change the RTF version
to the latest.

## Step 5: Edit the script class <a name="edit-test-script-class"></a>
Open the Test script class and call the **executeShell(String
shellFilePath)** method where the Shell script needs to be executed.

* The argument to this method is the shell file path, which is provided in the Element Repository file.
	
* The job yaml file path is provided in the element Element Repository file.

* The log directory can be provided if required and the key for log directory is "LOG_FILE=" followed by the value.

* The sleep time can be provided if required and the key for sleep time is "SLEEP_TIME=" followed by the value.

* ![](./images/media/image1.png)

## Step 6: Add the properties <a name="add-properties-elementrepo"></a>
Add the properties required for execute shell to read the
properties from element repository in the constants file.

![](./images/media/image2.png)

## Step 7: Update config.yml <a name="update-config.yml"></a>
Update the config.yml file in .circleci as in the rtf blank project

## Step 8: Update the docker file <a name="update-docker-file"></a>
Also update the Dockerfile as in the rtf blank project

## Step 9: Update the rapidTestConfig.properties <a name="update-rapidTestConfig-properties"></a>

* Modify RTF db configuration sections in the below format.

	spring.rtf.url=jdbc:postgresql://\${RTF_POSTGRES_HOST:localhost}:\${RTF_POSTGRES_PORT:5433}/\${RTF_POSTGRES_DATABASE:postgres}

	spring.rtf.username=\${RTF_POSTGRES_USER:users}

	spring.rtf.password=\${RTF_POSTGRES_PASSWORD:password}

	spring.rtf.platform=postgresql

## Step 10: Create a job.yml <a name="create-job.yml"></a>

Should create a job.yml for the RTF job about to run. A sample is
given.

Replace blank project contents mentioned below with that of your
project in job.yml.

metadata.name
metadata.labels.app
spec.template.metadata.labels.app
spec.template.spec.containers.name
spec.template.spec.containers.image

Add the environment variables required for your project after line 59
in job.yml in the same format of above variables.

*NOTE: DO NOT MAKE ANY CHANGES TO THE ENVIRONMENT VARIABLES ON OR
ABOVE LINE 59.*
