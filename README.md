## Sources for the Apache Airflow official Docker image

This is a read-only repository Containing Apache Airflow Dockerfile and corresponding
scripts that are needed to build the Official Airflow production-ready Docker/Container image.

This repo keeps only the necessary files in branches synchronized with the main
Apache Airflow repository:

* `master` - development branch where currently 2.0 is being developed
* `v1-10-stable` - the branch where committers of Apache Airflow release and tag
  sources corresponding to the official releases of Apache Airflow.

You can check out any tag in those branches and follow instructions found in the `README.rst` file found
after checkout to build the corresponding Docker container image: either from the PyPI packages or
via using convenience binary .whl packages released via
[Airflow Downloads](https://downloads.apache.org/airflow/).

If you want to make your own modifications to the Dockerfile - you should go to
[Apache Airflow](https://github.com/apache/airflow) repository where the Dockerfile is maintained and there
are all the test tools that allow to verify the image before your Pull Requests get merged. This repo
is simply read-only partial extract of the Apache Airflow repository.

Note that you can use the master/v1-10-stable Dockerfile to build any 1.10 Airflow version. It depends on the
build arguments used when running `docker build` command. However, the master version is a development
only branch and might contain not only fixes but also unresolved-yet problems). The HEAD of
v1-10-stable branch and tags corresponding to the actual Airflow version is the most stable solution.

Note! Currently, this change has not yet been released in v1-10-stable or official release of Airflow
so in order to build Airflow image (in any version), you need to checkout master branch. This will
change as of 1.10.13 version, and you will be able to checkout either `1.10.13` tag or `v1-10-stable` branch
and build Apache Airflow from there.

## Example of building the image

An example of how you can use this repository to build the image is below. It uses the default
configuration of the image to build it, but you can add your own build arguments to extend or build
the image. The command below prepares a Python 3.7 version of the image.

```shell script
git clone git@github.com:apache/airflow-docker.git

AIRFLOW_VERSION=1.10.12
readonly AIRFLOW_VERSION

# Note that currently you need to release from master as the change has not yet been merged into
# v1-10-stable branch and has not yet been released in any Airflow version. It will be possible to checkout
# the released version as of Airflow 1.10.13
git checkout master

docker build . \
    --build-arg PYTHON_BASE_IMAGE="python:3.7-slim-buster" \
    --build-arg PYTHON_MAJOR_MINOR_VERSION=3.7 \
    --build-arg AIRFLOW_INSTALL_SOURCES="apache-airflow" \
    --build-arg AIRFLOW_INSTALL_VERSION="==${AIRFLOW_VERSION}" \
    --build-arg AIRFLOW_CONSTRAINTS_REFERENCE="constraints-1-10" \
    --build-arg AIRFLOW_SOURCES_FROM="empty" \
    --build-arg AIRFLOW_SOURCES_TO="/empty"

```

You can build the image using different arguments and parameters and it is described in the `README.rst` file
that you will see when you checkout the master/v1-10-stable.



## How the repository synchronization works

We have a GitHub Workflow that runs periodically (hourly) and keeps the relevant branches in sync with the
Apache Airflow repository. The script only keeps files and commits relevant to the Dockerfile building though.
This is done thanks to fantastic [git-filter-repo](https://github.com/newren/git-filter-repo)
project that allows for automation of branch and path filtering.

## Official release disclaimer

Note that this repository is just a convenience one. It is maintained by the Apache Airflow community, and
you can clone and check it out if you wish, but the only official sources of Apache Airflow releases
are at [Airflow Downloads](https://downloads.apache.org/airflow/) where you can find the .tar.gz
sources of the releases which have been officially voted on by PMC and released following the
[ASF Release Policy](https://www.apache.org/legal/release-policy.html). The person acting as release manager
of the Apache Airflow signs and verifies those packages after they have been voted on.
