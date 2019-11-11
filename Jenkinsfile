pipeline {
    agent any

    environment {
        IMAGE_NAME = "mymaven" //<1>
    }

    stages {
        stage("Build docker images") {
            steps {
                script {
                    def versions = readYaml file: "versions.yml" //<2>

                    def stages = versions.images.collectEntries { label, props -> //<3>
                        [(label): {
                            stage(label) {
                                sh """docker build \
                                --build-arg JAVA_VERSION=${props.java} \
                                --build-arg MAVEN_VERSION=${props.maven} \
                                ${props.tags.collect { tag -> "-t ${env.IMAGE_NAME}:${tag}" }.join(" ") } \
                                .
                            """
                            }
                        }]
                    }

                    parallel stages //<4>
                }
            }
        }
    }
}