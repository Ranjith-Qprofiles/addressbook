pipeline{
    agent any 
    stages{
        stage("Installed Maven Build")
        {
            steps
            {
                echo "Installed Maven Build"
                //sh 'tar -xvzf /var/lib/jenkins/workspace/test/apache-maven-3.9.4-bin.tar.gz'
            }
        }
        stage("Compiling Addressbook Application")
        {
            steps
            {
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                     sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn compile'
                }
            }
        }
        stage("Test Addressbook Application")
        {
            steps
            {
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                    sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn test'
                }
            }
        }
        stage("Package Addressbook Application")
        {
            steps
            {
                dir('/var/lib/jenkins/workspace/addressbook_pipeline_job/addressbook/addressbook_main')
                {
                    sh '/var/lib/jenkins/workspace/addressbook_pipeline_job/apache-maven-3.9.4/bin/mvn package'
                }
            }
        }
        stage("Tomcat Prerequisites Installation")
        {
            steps
            {
                sh 'sudo apt update'
                sh 'sudo apt install openjdk-17-jre -y'
            }
        }
    }
}
