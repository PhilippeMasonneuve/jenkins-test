pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Trying to build the other pipeline'
                
                
                script{
                    def workers_dir =  "./folder"
                    def  FILES_LIST = sh (script: "ls   '${workers_dir}'", returnStdout: true).trim()
                    def full = ""
                    for (String elem: FILES_LIST.split("\\r?\\n")){
                        path = "./folder/"+elem
                        elem = "${elem}:${readFile(file: path)}"
                        full = full + elem + "/"
                        echo elem
                    }
                    // //PARSING
                    // echo full
                    // build job: 'pipe',
                    // parameters: [
                    //     string(name: 'MESSAGE', value:full)
                    // ]
                    writeFile file: ".s/usefulfile.txt", text: "This file is useful, need to archive it."
                }
            }
        }
        stage("Commit") {
            steps {
                sh('''
                    git checkout -B master
                    git config user.name 'philippMasonneuve'
                    git config user.email 'philippe.maisonneuve@polymtl.ca'
                    git add . && git commit -am "[Jenkins CI] Add build file"
                ''')
            }
        }
        stage("Push") {
            echo "act 1"
            environment { 
                    GIT_AUTH = credentials('support-team-up') 
                }
            echo "act 2"
                steps {
                    sh('''
                        git config --local credential.helper "!f() { echo username=\\$GIT_AUTH_USR; echo password=\\$GIT_AUTH_PSW; }; f"
                        git push origin HEAD:master
                    ''')
                }
            echo "act 3"
            }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
