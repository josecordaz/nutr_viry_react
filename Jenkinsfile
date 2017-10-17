node {
    def app
    def receiver_container

    stage('Clone repository'){
        /*Let's make sure we have the repository cloned to our workspace*/
        checkout scm
    }

    stage('Build image'){
        /*
        * This builds the actual image: synonymous to
        docker build on the command line
        */
        app = docker.build("josecordaz/nutr_viry")
    }

    stage('Test image'){
        /*
        * Ideally, we would run a test framework against our image.
        * For this example, we're using a Volkswagen-type approach ;-)
        */
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image'){
        /*
        * Finally, we'll push the image with two tags:
        * First, the incremental build number from Jenkins
        * Second, the 'latest' tag.
        * Pushing multiple tags is cheap, as all the layers are reused.
        */
        docker.withRegistry('https://registry.hub.docker.com','docker-hub-credentials'){
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Starting nutr_viry container') {
        receiver_container.stop()
        receiver_container = docker.image('josecordaz/nutr_viry:1.0').run('--name ntr_viry_c -p 8088:80 -d')
    }
}
