pipeline{
    agent any
    environment {
        ANYPOINT = credentials('dan-anypoint-runtime')
    }
    stages {
        stage ('Build') {
            steps {
                withMaven(maven: 'maven_3.6.3') {
                    configFileProvider([configFile(fileId: '10b55527-bbec-48ac-ba7f-1cad3e604701', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS_XML')]) {
                            sh """mvn -U -s $MAVEN_SETTINGS_XML clean compile"""
                    }
                }
            }
        }
        stage('Test') {
            steps {
                withMaven(maven: 'maven_3.6.3') {
                    configFileProvider([configFile(fileId: '10b55527-bbec-48ac-ba7f-1cad3e604701', targetLocation: 'settings.xml', variable: 'MAVEN_SETTINGS_XML')]) {
                            sh """mvn -U -s $MAVEN_SETTINGS_XML clean verify -DskipTests"""
                    }
                }
            }
        }
        stage ('Deploy'){
            steps {
                withMaven(maven:'maven_3.6.3'){
                    sh 'mvn package deploy -DattachMuleSources -Denvironment=Dev -DworkerType=MICRO -DconnectedAppClientId=$ANYPOINT_USR -DconnectedAppClientSecret=$ANYPOINT_PSW -DconnectedAppGrantType=client_credentials -DmuleDeploy -DskipTests'
                }
            }
        }
        stage ('clean') {
            steps {
                cleanWs()
            }
        }
    }
}
