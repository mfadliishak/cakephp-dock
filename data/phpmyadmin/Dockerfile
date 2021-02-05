# Base Image
FROM phpmyadmin/phpmyadmin:5.0.1

# Update and Installation
RUN apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y git && \
  rm -rf /var/lib/apt/lists/*