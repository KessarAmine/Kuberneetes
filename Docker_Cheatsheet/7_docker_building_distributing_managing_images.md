# Building images
additional tips

docker image build -t <IMAGE>:<TAG>
    -f <DOCKERFILE_NAME>
    --force-rm : always remove intermediate containers craeted during the exec of the layers of Dockerfile
    --label : adding some metadata to an image
    --rm : removes those intermediate containers only after success of image build
    --ulimit: ulimit options
. : <PATH_DOCKERFILE>

piping the dockerfile through STDIN:
docker image build -t <NAME>:<TAG>
-<<EOF
/*BUILD INSTRUCTIONS*/
EOF (ending the piping using the delemeter EOF just like in heredoc)

using a remote URL:
docker image build -t <NAME>:<TAG> <GIT_URL>\
    #<REF> (name if the branche)
    #:<DIRE> (name of the dire)
    #<REF>:<DIRE> (name of the dire in the branch)

using a tar file
docker image build -t <NAME>:<TAG>
    - < <FILE>.tar.gz

# Buidling multi-stage images
this in one way of ensuring to keep our images as small as possible, so we will be using multi images to build a final product, one single dockerfile in which we can define multiple images inside
first stage is in which we download code, indepencies installation and test our code, second one is the product
copying the build artefact in different build stages we eleminate the intermidiate steps, which will alow us to get rid of all those additional layers form the final image
by the defautl the stages are not named, they are integer numbers, 0 for the first FROM instruction
we can name the stage by adding <NAME> to the FORM instruction and then reference the name in the COPY instruction

Dockerfile:
// our build stage 0 image
FROM node <AS BUILD>
/*
instruction of downloading all the code and the depandacies installation
*/

// the second stage that we want, final product image
FROM node:alpine
...
// COPY --from=<FIRST_STAGE_IMAGE> <PATH_SOURCE> <PATH_DESTINATION>
COPY --from=build /var/node /var/node
...

# TAGGING
a good way to keep track between the version fo the images and the version of the code is to use the HASH of the commits of the code as the images tags, in other words, run:
git log -1 --pretty=%H
docker image build -t <NAME>:<PASTE_COMMIT_HASH> .

# Distributing iamges
if not already logedin, execute:
    docker login
all images will be pushed to dockerhub
we use docker image push, after an image is already craeted in the local machine:
    docker image push <USERNAME>/<IMAGENAME>:<TAG>
to ensure tracking of the latest version, after pushing the image with the commit_has as tag, we push an other copy as the latest "tag"
we can do both of the steps here:
docker image tag [USERNAME]/[IMAGE_NAME]:[HASH] [USERNAME]/[IMAGE_NAME]:latest

# IMAGE HISTORY
we can get insights how an image is built, list layers created with ids and commands
docker image history <NAME>:<TAG>
docker image history --quiet --no-trunc <NAME>:<TAG>

# SAVING LOADING IMAGES
export images as .tar files

docker image save <IMAGE> 
    (> || -o || --output)
     <FILE>.tar
gzip <FILE>.tar to compress it = <FILE>.tar.gz
if we wanna save the binaries we need to use ci/cd tool like jinkens

import images as .tar files

docker image load 
    (< || -i || --input)
    <FILE>.tar