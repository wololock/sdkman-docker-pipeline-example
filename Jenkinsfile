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
                                ${collectTags(props.tags, env.IMAGE_NAME)} \
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

@NonCPS
String collectTags(final List<String> tags, final String imageName) {
    return tags.collect { tag -> "-t ${imageName}:${tag}" }.join(" ")
}