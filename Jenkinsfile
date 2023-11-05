pipeline{
    agent any 
    //Declaration of Environment Variables
    environment{
        MAVEN_VERSION='Apache Maven 3.6.3'     
        APACHE_TOMCAT='Apache-Tomcat-8.5.24'
    }
    //Allows the user to provide parameters for a build  
    parameters {
        choice(choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Select the version to build', name: 'VERSION')
        booleanParam(defaultValue: true, description: 'executeMavenStage checked then Execute Installed Maven Build Stage ', name: 'executeMavenStage')
    }
    stages{
        //Parallel Stages are (Installed Maven Build,Compiling Application,Testing Application,Packaging Application)
        stage("Parallel Stages")
        {
            parallel
            {
        stage("Installed Maven Build")
        {
            //When I checkbox "executeMavenStage" then only execute this stage
            when{
                expression{
                    params.executeMavenStage
                }
            }
           steps 
            {
                echo "Installed Maven Build"
                echo "Maven VERSION is $MAVEN_VERSION"
                sh 'wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz'
                sh 'tar -xvzf /var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4-bin.tar.gz'
            }
        }
        stage("Compiling Application")
        {
            steps
            {   
               //Go Inside directory, compile the code 
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                     sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn compile'
                }
                //echo "Compiling Application"
            }
        }
        stage("Testing Application")
        {
            steps
            {
                //Go Inside directory, test the code
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                    sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn test'
                }
               // echo "Testing Application"
            }
        }
                
        stage("Packaging Application")
        {
            steps
            {
                //Go Inside directory, package the code
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                    sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn package'
                }
                //echo "Build War File"
            }
        }
            }
        }
        stage("Tomcat Prerequisites Installation")
        {
            steps
            {
                //Tomcat Prerequisites Installation
                sh 'sudo apt update'
                sh 'sudo apt install openjdk-17-jre -y'
            }
        }
        stage("Install Apache Tomcat Server")
        {
            steps
            {
                //Installing Apache-tomcat-8.5.24
                sh 'wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz'
                sh 'tar -xzvf apache-tomcat-8.5.24.tar.gz'
                echo 'Tomact Version is $APACHE-TOMCAT'
                sh 'sudo chown -R ubuntu:ubuntu apache-tomcat-8.5.24'
                sh 'sudo cp tomcat-users.xml apache-tomcat-8.5.24/conf/tomcat-users.xml'
                sh 'sudo cp context.xml apache-tomcat-8.5.24/webapps/manager/META-INF/context.xml'
                sh 'sudo sed -i "s/8080/8081/g" apache-tomcat-8.5.24/conf/server.xml'
            }
        }
        stage("Deploy WAR file to Tomcat Server")
        {
            steps
            {
                //Copy WAR FROM addressbook/addressbook_main/target/addressbook.war TO apache-tomcat-8.5.24/webapps
                sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-8.5.24/webapps'
                //by using mentioned sudeors user "ubantu" Start Apache Tomcat Server 
                sh 'sudo runuser -l ubuntu -c "/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-tomcat-8.5.24/bin/startup.sh"'
            }
        }
    }
    post
    {
        //Trigger Email when BUILD SUCCESS
        success{
            mail to: "ranjithkumark786@gmail.com",
            subject: "Jenkins build is back to normal: $JOB_NAME $BUILD_DISPLAY_NAME",
            body :  "see '<$JOB_URL/$BUILD_DISPLAY_NAME/>'"
        }
        //Trigger Email when BUILD FAILURE
        failure{
            mail to: "ranjithkumark786@gmail.com",
            subject: "Build failed in Jenkins: $JOB_NAME $BUILD_DISPLAY_NAME",
            body : "see '<$JOB_URL/$BUILD_ID/console>'"
        }
    }
}
