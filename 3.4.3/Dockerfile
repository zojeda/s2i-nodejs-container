FROM centos/s2i-base-centos7

# This image provides a R environment you can use to run your R
# applications.

EXPOSE 8080

# This image will be initialized with "Rscript -e 'source(Sys.getenv("R_MAIN"))'"
# Description
# Environment:
# * $R_MAIN - Select an alternate / custom shiny application to run.
# Expose ports:
# * 8080 - Unprivileged port used by r/shiny application

ENV R_VERSION=3.4.3 \
    R_MAIN=main.R \
    NAME=R \
    R_INSTALL_DEPENDENCIES=install_dependencies.R

ENV SUMMARY="Platform for building and running R $R_VERSION applications" \
    DESCRIPTION="R $R_VERSION available as docker container is a base platform for \
building and running various R $R_VERSION applications."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="R $R_VERSION" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,$NAME,$NAME$R_VERSION" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i" \
      io.s2i.scripts-url="image:///usr/libexec/s2i" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.deployments-dir="${APP_ROOT}/src" \
      com.redhat.component="rh-$NAME$R_VERSION-docker" \
      name="centos/$NAME-$R_VERSION-centos7" \
      version="$R_VERSION" \
      maintainer="Zacarias F. Ojeda <zojeda@gmail.com>" \
      help="For more information visit https://github.com/zojeda/s2i-r-shiny-container" \
      usage="s2i build <SOURCE-REPOSITORY> centos/$NAME-$R_VERSION-centos7:latest <APP-NAME>"

RUN yum install -y centos-release-scl-rh epel-release && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    INSTALL_PKGS="R-3.4.3" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image, including help file
COPY ./root/ /

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 ${APP_ROOT} && chmod -R ug+rwx ${APP_ROOT} && \
    rpm-file-permissions

USER 1001

# Set local user library path
RUN mkdir -p ${APP_ROOT}/R/library
RUN echo 'R_LIBS_USER="${APP_ROOT}/R/library"' > $HOME/.Renviron
RUN R -e "install.packages(c('shiny', 'rmarkdown'), repos='https://cran.rstudio.com/')"
# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
