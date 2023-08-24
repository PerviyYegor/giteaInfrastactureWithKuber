# Use base alpine image
FROM alpine:latest

ARG TOKEN
ARG REPO 
RUN test -n "$TOKEN"
RUN test -n "$REPO"

# install git and xz-utils
RUN apk update && apk add --no-cache bash git xz git github-cli

# Set env variables
#ENV giteaReleaseUrl=https://github.com/go-gitea/gitea/releases/download/v1.20.0-rc0/gitea-1.20.0-rc0-linux-amd64
#ENV giteaReleaseName=gitea-1.20.0-rc0-linux-amd64
#RUN wget "$giteaReleaseUrl" -P /tmp 


RUN echo "$TOKEN" | cut -d ':' -f 2 > token.txt

# Install binary of gitea from private GitHub release
RUN gh auth login --with-token < token.txt 
RUN rm token.txt

RUN echo "last gitea release is version $(gh release list -R $REPO| awk '$2 == "Latest" {print $3}') in $REPO. Build image with this."
RUN gh release download -R $REPO $(gh release list -R $REPO| awk '$2 == "Latest" {print $3}') -D /tmp

# Add a git user
RUN adduser -D "git" && echo "git:git" | chpasswd -e

# Create data directory
RUN mkdir -p "/home/git/app/" \
    "/home/git/app/data" \
    "/home/git/app/custom" \
    "/home/git/app/custom/conf" \
    "/home/git/app/custom/https" \
    "/home/git/app/custom/mailer" \
    "/home/git/app/indexers" \
    "/var/log/gitea" \
    && touch gitea.log \
    && chown -R "git:git" "/home/git/app/" \
    && chmod -R u=rwX,g=rX,o= "/home/git/app/"

WORKDIR "/home/git/app/"

# Propagate gitea binary
RUN mv "/tmp/gitea" "/home/git/app/gitea"
RUN chmod 0755 "/home/git/app/gitea" \
    && chown "git":"git" "/home/git/app/gitea"

# Clean package manager cache
RUN rm -rf /var/cache/apk/*
USER git

# Copy file "app.ini" in "custom/conf/app.ini" if this exist
COPY Dockerfile app.ini* ./custom/conf/
RUN  rm -f ./custom/conf/Dockerfile


# Add the command to remove the directory
CMD ["sh", "-c", "rm -fr /home/git/app/data/queues/common && ./gitea web"]
