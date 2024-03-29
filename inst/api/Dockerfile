# Base image https://hub.docker.com/u/rocker/
FROM rocker/shiny:latest

# system libraries of general use
## install debian packages
RUN apt-get update -qq && apt-get -y --no-install-recommends install \
    libxml2-dev \
    libcairo2-dev \
    libsqlite3-dev \
    libmariadbd-dev \
    libpq-dev \
    libssh2-1-dev \
    unixodbc-dev \
    libcurl4-openssl-dev \
    libssl-dev

## Update system libraries
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean && \
    apt-get autoclean

## GIS tools
RUN apt-get -y update && apt-get -f install -y \
    default-libmysqlclient-dev \
    libmysqlclient-dev \
    libudunits2-dev \
    libgdal-dev \
    libgeos-dev \
    libproj-dev


## Install R packages from CRAN:
RUN R -e "install.packages('remotes', repos = 'http://cran.us.r-project.org')" -e "install.packages('RSQLite', repos = 'http://cran.us.r-project.org')" -e "install.packages('sf', configure.args = '--with-proj-include=/proj_home/include --with-proj-lib=/proj_home/lib --with-proj-api=no')" -e "remotes::install_github('cjcallag/ps50sr', force = TRUE)"

## Expose port
EXPOSE 3838
EXPOSE 591
EXPOSE 8080

# Set non-root
RUN useradd shiny_user
USER shiny_user

# Run app on container start
CMD ["R", "-e", "ps50sr::launch_api(docs = FALSE, port = 591)"]

# Running the Dockerfile
## cd inst/api/
## docker build -t csg-api-image . --no-cache
## docker run -d --rm -p 591:591 csg-api-image
