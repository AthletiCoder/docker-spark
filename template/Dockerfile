FROM bde2020/spark-submit:3.0.1-hadoop3.2

LABEL maintainer="Varun Kumar Nagandla <varun.nagandla@ril.com>"

ARG SBT_VERSION
ENV SBT_VERSION=${SBT_VERSION:-1.4.1}

RUN wget -O - https://github.com/sbt/sbt/releases/download/v${SBT_VERSION}/sbt-${SBT_VERSION}.tgz | gunzip | tar -x -C /usr/local

ENV PATH /usr/local/sbt/bin:${PATH}
RUN mkdir -p /app/project/spark_test/project

WORKDIR /app/project/spark_test

# Pre-install base libraries
ADD build.sbt /app/project/spark_test
ADD plugins.sbt /app/project/spark_test/project
RUN touch /app/project/spark_test/project/build.properties
ADD project /app/project/spark_test
RUN sbt update

COPY template.sh /

ENV SPARK_APPLICATION_MAIN_CLASS HelloWorld

# Copy the build.sbt first, for separate dependency resolving and downloading
# ONBUILD COPY build.sbt /app/project/spark_test
# ONBUILD COPY project /app/project/spark_test
# ONBUILD RUN sbt update

# Copy the source code and build the application
ONBUILD COPY . /app
RUN sbt clean assembly

CMD ["/template.sh"]
