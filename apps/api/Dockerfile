# this needs to be at the top, what version are we building
ARG IMAGE_VERSION="latest"


# --------------------------------------------------------------------------
# PECAN FOR MODEL BASE IMAGE
# --------------------------------------------------------------------------
FROM pecan/base:${IMAGE_VERSION} 
LABEL maintainer="Tezan Sahu <tezansahu@gmail.com>"

EXPOSE 8000

# --------------------------------------------------------------------------
# Variables to store in docker image (most of them come from the base image)
# --------------------------------------------------------------------------

# COMMAND TO RUN
RUN   apt-get update \
  &&  apt-get install libsodium-dev -y \
  &&  rm -rf /var/lib/apt/lists/* \
  &&  Rscript -e "devtools::install_version('promises', '1.1.0', repos = 'http://cran.rstudio.com')" \
  &&  Rscript -e "devtools::install_version('webutils', '1.1', repos = 'http://cran.rstudio.com')" \
  &&  Rscript -e "devtools::install_github('rstudio/swagger')" \
  &&  Rscript -e "devtools::install_github('rstudio/plumber')"

ENV AUTH_REQ="TRUE" \
    HOST_ONLY="FALSE" \
    PGHOST="postgres"\
    RABBITMQ_URI="amqp://guest:guest@rabbitmq/%2F"\
    DATA_DIR="/data/"\
    DBFILES_DIR="/data/dbfiles/"
    
WORKDIR /api/R

CMD Rscript entrypoint.R

COPY ./ /api