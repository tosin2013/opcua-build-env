# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork, which includes standalone examples
RUN git clone https://github.com/eclipse/milo.git

# Go into directory that was checked out and use maven to build a server jar
# RUN cd milo/opc-ua-stack && mvn package -P standalone -Dmaven.test.failure.ignore=true && cd /app
RUN cd milo && mvn clean install -DskipTests && cd /app

RUN ls -R /app

# Copy the directory contents of the milo's directory into the container at /app
RUN cp  milo/milo-examples/server-examples/target/server-examples-0.6.11-SNAPSHOT.jar /app

# Add seperate directory for secrets that should not be copied into image for reuseability
VOLUME /app/certificates

# Tell the server it is responsible for traffic adressed to "server", docker networks substitute the correct IP
ENV OPCUA_SERVER_IP "server"

# Here the certificates for the can be found, both the server's own and rejected or trusted client certificates
ENV OPCUA_CERT_DIR "/app/certificates"

EXPOSE 4840

# Run example client
CMD ["java", "-jar", "server-examples-0.6.11-SNAPSHOT.jar"]