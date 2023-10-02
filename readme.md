# Jenkins
  - its a open source tool for CI/CD of software projects
      - CI - Continuous Integration (Preparation to Before Build)
       - CD - Continuous deployment (Build & Deploy)
  - it automates build & deployment process of softwares
  
# Process
  - Preparation > Test > Security/Code Scan -> Build -> Deploy  

# Installation 
  - Approach 1 : Manual Installation ( requires many softwares like  
                 java & all)
  - Approach 2 : Using Docker ( we don't need to take much efforts, by 
                 pass complexity )
                 - in a container we can run jenkins

# Jenkins with Docker
    - will use jenkins as base image
    - on the top will use docker in the container
    - Create Dockerfile.jenkins
    - Create jenkins-dashboard.yaml
    - Run : docker-compose -f jenkins-dashboard.yaml  up -d
    - Wait for Process completion
    - Once Installation done , got to port 8080
    - Here for unlock jenkins we need to enter token which will get from logs

# Create JOB/ITEM/PROJECT
    - The tasks which will be perform by jenkins together called JOB


# Hello Word Print Job
   - Create Job 
   - Enter Job Name 
   - Select Free Style -> Ok
   - Build --> Add Shell Script -> echo "hello world"
   - Build Now
   - You will see build done 
   - Check console output ,here will see "hello world"

# Sample Node JS Application from Git (Manual)
   - keep a sample repo on git so we can close and run
   - Create Job with free style for as of now
   - then source code management , choose git , add repo 
   - now build steps
      - execute shell
        - npm install | but here npm will not recognise by jenkins 
           - here we need to use plugin
           - steps
             1. Go to dashboard > manage jenkins
             2. plugins >  available plugins > search 'node' > will get node js
             3. Select Node Js > Install > Download > Restart Jenkins Container
             4. Once its install then go to manege jenkins > tools (configure which node we need to use)
             5. Go to Node Js install > Choose Version (for now 16X), then install automatically should tick , then save
             6. Go back to node js job > Configure (for configure node)
             7. will notice in build env > 'Provide Node & npm bin/ folder to PATH'
             8. Select'Provide Node & npm bin/ folder to PATH' this , in script should be 'npm install" 
             9. Save
    - Build Now
    - if you want to image build & publish to docker hub | install cloudbees docker for this and add into build step
        - go to job
        - go to configure > add build steps
        - select docker build & publish
          -- repositry name | USERNAME/Project name eg : sachingupta477/node-js-jenkins-demo
          -- tag name | we can select env variable | ${BUILD_NUMBER}
          - save
          Note - but when you build it will not build & publish we need to login first 
          so update shell script
          ```
          npm install
          docker login
          ```
          - [Important] - docker login we need to provide credential , we can do into terminal but safe way to create a global credential in jenkins
          - also as password put accesstoken
          - go to job now > configure > build environment > use secret text > Give VARIABLE NAME for holding username & password
          - now go to shell script and update

          ```
          npm install
          docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
          ```
          - again add shell script  , once everything is done
          ```
          docker logout
          ```
          - Build Now 
          - once success verify on docker hub
    
# Sample Node JS Application from Git (Via Code )
  - above process we have done manually , but if we can manage via code then it will be really helpful
  - we need to use plugin | Job DSL
    -- its a groovi based script
    -- create in app folder like app>job dsl > nodedocker.groovy
    -- from documentation you can check 
    Note : JOB DSL will create a seed job which will create another JOB
  - step
       1. create seed project job "seed project demo" with free style
       2. install plugin - job dsl
       3. Restart jenkins
       4. Open Seed job > Build Steps > process job DSLs
       5. add dsl script from git 
          - job dsl/nodedocker.groovy
       6. Save ,
       7. Build 
       Note - if it get error , then we need to review from admin