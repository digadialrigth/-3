# -3
version: 2.1
jobs:
 build:
 # pre-built images: https://circleci.com/docs/2.0/circleci-images/
  docker:
  - image: circleci/node:10-browsers
   auth:
    username: mydockerhub-user
    password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
    - checkout
     - run:
     name: The First Step
      command: |
       echo 'Hello World!'
        echo 'This is the delivery pipeline'
         - run:
          name: Code Has Arrived
           command: |
            ls -al
             echo '^^^That should look familiar^^^'
              - run:
              name: Running in a Unique Container
              command: |node -v
               ls -al
               echo '^^^That should look familiar^^^'
                Run-With-Node:
                docker:
                - image: circleci/node:10-browsers
                auth:
                 username: mydockerhub-user
                  password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
                   steps:
                   - run:
                    name: Running In A Container With Node
                     command: |
                      node -v
                      Now-Complete:
                       docker:
                       - image: alpine:3.7
                        auth:
                         username: mydockerhub-user
                          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
                          steps:
                          - run:
                           name: Approval Complete
                            command: | echo 'Do work once the approval has completed'
                            echo 'Do work once the approval has completed'

workflows:
version: 2
 Example_Workflow:
 jobs:
 - Hello-World
 - I-Have-Code:
  requires:
   - Hello-World
        - Run-With-Node:
        requires:
        - Hello-World
         - Hold-For-Approval:
          type: approval
           requires:
            - Run-With-Node
             - I-Have-Code
             - Now-Complete:
              requires:
              - Hold-For-Approval

