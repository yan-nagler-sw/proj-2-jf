pipeline {
    agent any

    parameters {
        choice(
            name: 'test_type',
            choices: ['3', '1', '2'],
            description: ''
        )
    }

    environment {
        usr = 'yannagler'
        crs_dir = "/Users/${usr}/Desktop/dev-ops-course"
        env_dir = "${crs_dir}/env"
        py_dir = "${crs_dir}/py"

        py = "python3.9"
        pkgs_dir = "${py_dir}/venv/lib/${py}/site-packages"
    }

    stages {
        stage("handle-git") {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('* * * * *')])])
                }
                git 'https://github.com/yan-nagler-sw/proj-2.git'
            }
        }

        stage("handle-prereq") {
            when {
                expression {
                    params.test_type == '3'
                }
            }

            steps {
                echo "test_type: ${params.test_type}"

                echo "Running Python script: backend_testing_db.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
                    ${py} backend_testing_db.py
                '''

                echo "Copying Selenium WebDriver - Chrome..."
                sh '''
                    cp ${env_dir}/chromedriver .
                '''
            }
        }

        stage("run-py-rest-app") {
            steps {
                echo "Running Python script: rest_app.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    nohup $py rest_app.py &
//                    ${py} backend_testing_rest.py
                    ${py} backend_testing_db.py
                '''
            }
        }

        stage("run-py-web-app") {
            steps {
                echo "Running Python script: web_app.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    nohup ${py} web_app.py &
                    ${py} backend_testing_db.py
                '''
            }
        }

        stage("run-py-be-test") {
            when {
                expression {
                    params.test_type == '3'
                }
            }

            steps {
                echo "test_type: ${params.test_type}"

                echo "Running Python script: backend_testing.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    ${py} backend_testing.py
                    ${py} backend_testing_db.py
                '''
            }
        }

        stage("run-py-fe-test") {
            when {
                expression {
                    params.test_type == '3'
                }
            }

            steps {
                echo "test_type: ${params.test_type}"

                echo "Running Python script: frontend_testing.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    ${py} frontend_testing.py
                    ${py} backend_testing_db.py
                '''
            }
        }

        stage("run-py-comb-test") {
            steps {
                when {
                    expression {
                        params.test_type == '3'
                    }
                }

                echo "Running Python script: combined_testing.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    ${py} combined_testing.py
                    ${py} backend_testing_db.py
                '''
            }
        }

        stage("run-py-clean-env") {
            steps {
                echo "Running Python script: clean_environment.py..."
                sh '''
                    export PYTHONPATH="${pkgs_dir}:\\$PYTHONPATH"
//                    ${py} clean_environment.py
                    ${py} backend_testing_db.py
                '''
            }
        }
    }
}
