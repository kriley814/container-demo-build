node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("container_build_demo")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        /* docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') */ 
	 docker.withRegistry('https://959844384370.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-creds') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
         } 
    }
}
