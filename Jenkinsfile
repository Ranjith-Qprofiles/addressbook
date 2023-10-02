pipeline{
    agent any 
    environment{
        MAVEN_VERSION='Apache Maven 3.6.3'
        // APACHE-TOMCAT='Apache-Tomcat-8.5.24     
    }
    parameters {
        choice(choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Select the version to build', name: 'VERSION')
        booleanParam(defaultValue: true, description: 'Parameter Selected the Execute Maven Job ', name: 'executeMavenStage')
    }
    stages{
        stage("Installed Maven Build")
        {
            when{
                expression{
                    params.executeMavenStage
                }
            }
           steps 
            {
                echo "Installed Maven Build"
                echo "Maven $name is $MAVEN_VERSION"
                //sh 'tar -xvzf /var/lib/jenkins/workspace/test/apache-maven-3.9.4-bin.tar.gz'
            }
        }
        // stage("Compiling Application")
        // {
        //     steps
        //     {
        //         dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
        //         {
        //              sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn compile'
        //         }
        //     }
        // }
        // stage("Testing Application")
        // {
        //     steps
        //     {
        //         dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
        //         {
        //             sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn test'
        //         }
        //     }
        // }
        // stage("Packaging Application")
        // {
        //     steps
        //     {
        //         dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
        //         {
        //             sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn package'
        //         }
        //     }
        // }
        // stage("Tomcat Prerequisites Installation")
        // {
        //     steps
        //     {
        //         sh 'sudo apt update'
        //         sh 'sudo apt install openjdk-17-jre -y'
        //     }
        // }
        // stage("Install Apache Tomcat Server")
        // {
        //     steps
        //     {
        //         sh 'wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz'
        //         sh 'tar -xzvf apache-tomcat-8.5.24.tar.gz'
        //         echo 'Tomact Version is $APACHE-TOMCAT'
        //         sh 'sudo chown -R ubuntu:ubuntu apache-tomcat-8.5.24'
        //         sh 'sudo cp tomcat-users.xml apache-tomcat-8.5.24/conf/tomcat-users.xml'
        //         sh 'sudo cp context.xml apache-tomcat-8.5.24/webapps/manager/META-INF/context.xml'
        //         sh 'sudo sed -i "s/8080/8081/g" apache-tomcat-8.5.24/conf/server.xml'
        //     }
        // }
        // stage("Deploy WAR file to Tomcat Server")
        // {
        //     steps
        //     {
        //         sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-8.5.24/webapps'
        //         sh 'sudo runuser -l ubuntu -c "/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-tomcat-8.5.24/bin/startup.sh"'
        //     }
        // }
    }
    post
    {
        success{
            mail to: "ranjithkumark786@gmail.com",
            subject: "Jenkins build is back to normal: $JOB_NAME $BUILD_DISPLAY_NAME",
            body :  "see '<http://13.53.35.96:8080/job/addressbook_pipeline_job/$BUILD_DISPLAY_NAME/>'"
        }
        failure{
            mail to: "ranjithkumark786@gmail.com",
            subject: "Build failed in Jenkins: $JOB_NAME $BUILD_DISPLAY_NAME",
            body : "see '<http://13.53.35.96:8080/job/addressbook_pipeline_job/$BUILD_ID/console>'"
        }
    }
}
