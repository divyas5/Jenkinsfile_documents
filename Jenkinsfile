***Standard syntax of pipeline
pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                // 
            }
        }
        stage('Test') { 
            steps {
                // 
            }
        }
        stage('Deploy') { 
            steps {
                // 
            }
        }
    }
}

-------------------------------------------------------------
***Completed Python example***
pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deploy code') {
            agent any
            environment { 
                VOLUME = '$(pwd):/src'
                IMAGE = 'cdrx/pyinstaller-linux:python3'  // pyinstaller-linux:python2 -> pyinstaller-windows:python3
            }
            steps {
                dir(path: env.BUILD_ID) { 
                    unstash(name: 'compiled-results') 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F sources/add2vals.py'" 
                }
            }
            post {
                success {
                    archiveArtifacts "${env.BUILD_ID}/dist/add2vals" 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
                }
            }
    }
}
-----------------------------------------------------------
***Simple Pipeline***

pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Cleanup stage') { 
            steps { 
                bat 'echo Cleanup stage' 
            }
        }
        stage('Git checkout'){
            steps {
                bat 'echo Git checkout'
            }
        }
        stage('Restore package stage') {
            steps {
                bat 'echo Restore package'
            }
        }
        stage('Build stage'){
            steps {
                bat 'echo Build'
            }
        }
        stage('Test execution stage'){
            steps {
                bat 'echo Test execution started'
                bat 'echo Test execution completed'
            }
        }
    }
}

---------------------------------------------------------------------------------
***Build Chaining using freestyle job***

Job 1. echo 'Build first stage' (Excute on batch command) in build.....In post build action (Job 2) click on "Trigger only if build is stable"
Job 2. echo 'Build Second stage' (Excute on batch command) in build.....In post build action (Job 2) click on "Trigger only if build is stable"
Job 3. echo 'Build Third stage' (Excute on batch command) in build.....In post build action (Job 2) click on "Trigger only if build is stable"

Click on + button where in pipeline flow select Based on upstream/downstream relationship and select initial job like Job 1

------------------------------------------------------------------------------------
***Copy and paste file from one folder to another***

bat file save as .bat in your local machine:- 
@ECHO OFF

set Source=%1
set Destination=%2
copy %Source% %Destination%

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
               bat 'C:\\Users\\divyas5\\Downloads\\copyfiles.bat C:\\test D:\\test1'
               
            }
        }
    }
}

here mention your .bat file location first and then source file location and destination file location 

--------------------------------------------------------------------------------------------
***used docker in local machine in freestyle project***
Put this command on windows batch script but keep sure installed docker in your local machine
docker build -t test .
docker run test:latest

-------------------------------------------------------------------------------------------
***check python docker image version***
pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                sh 'echo check'
                sh 'python --version'
            }
        }
    }
}
where put your Dockerfile and Jenkinsfile

-------------------------------------------------------------------------------------------
***Used created node and run batch script pipeline***
pipeline {
    agent { label 'test-node' }
    stages {
        stage('git') { 
            steps {
               git credentialsId: 'a7fcf27b-4a62-448e-89e2-4895f6f0ef14', url: 'https://github.com/divyas5/JavaPrograms.git'
               bat 'git.bat'
          
            }
        }
    }
}

in .bat file mention :- "C:\Users\divyas5\AppData\Local\Programs\Python\Python38\python.exe" "C:\Program Files (x86)\Jenkins\workspace\python-github\hello.py"
and .py file mention :- print ('Hello')
put file in git :- Jenkinsfile,.bat file,.py file

--------------------------------------------------------------------------------------------------------------
***	Write batch script for print hello message (python)***
pipeline {
    agent any
    stages {
        stage('git') { 
            steps {
               git credentialsId: 'a7fcf27b-4a62-448e-89e2-4895f6f0ef14', url: 'https://github.com/divyas5/JavaPrograms.git'
               bat 'git.bat'
          
            }
        }
    }
}

in .bat file mention :- "C:\Users\divyas5\AppData\Local\Programs\Python\Python38\python.exe" "C:\Program Files (x86)\Jenkins\workspace\python-github\hello.py"
and .py file mention :- print ('Hello')
put file in git :- Jenkinsfile,.bat file,.py file

------------------------------------------------------------------------------------------------------------
***Write a dockerfile and its output shown in a jenkins pipeline***
pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                sh 'echo check'
                sh 'python --version'
                sh 'python myscript.py'
            }
        }
    }
}

put jenkinsfile, dockerfile, .py file
* dockerfile :-
FROM python:3
ADD myscript.py /
CMD [ "python", "myscript.py" ]
* .py file (myscript)
print ('Hello')

-----------------------------------------------------------------------------------------------------------------------
***python code run using docker image without using docker file

pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker{
                   image 'python:3.8-alpine'
                }
            }
            steps {
                sh 'python myscript.py'
            }
            
        }
    }
} 

put jenkinsfile and python file

---------------------------------------------------------------------------------------
***Run python code on docker python image*** same as upper only python image and sh command change here
pipeline {
    agent none 
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:latest' 
                }
            }
            steps {
                sh 'python -m py_compile hello.py' 
            }
        }
    }
}
put Jenkinsfile and python file.

------------------------------------------------------------------------------------------------
***SMTP email notification***

----------------------------------------------------------------------------------------------
***


pipeline {
    agent none
    stages {
        stage('Build code') {
            agent {
                docker {
                    image 'python:3.8-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        
        stage('Deploy code') {
            agent any
            environment { 
                VOLUME = '$(pwd):/src'
                IMAGE = 'cdrx/pyinstaller-linux:python3'  // pyinstaller-linux:python2 -> pyinstaller-windows:python3
            }
            steps {
                dir(path: env.BUILD_ID) { 
                    unstash(name: 'compiled-results') 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller --onefile sources/add2vals.py'" 
                }
            }
            post {
                success {
                    archiveArtifacts "${env.BUILD_ID}/dist/add2vals" 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
                    //sh "docker build -t calculator"
                    //sh "docker run -i calculator" 
                }
            }
        }
        
/*      stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com/repository/docker/prajaktak202/calculator_application','dockerhub') {
                        def customImage = docker.build("my-image:${env.BUILD_ID}")
                        customImage.push()
                    }
                }
            }      
        }
*/
    }
}