FROM python:3.12.0-slim-bookworm

WORKDIR /app

# Install dependencies for container
RUN apt-get update -y && apt-get install --no-install-recommends -y wget jq git gcc libc6-dev libmagic1 && rm -rf /var/lib/apt/lists/*

# Define versions
ARG BACKEND_VERSION=0.2.0
ARG FRONTEND_VERSION=0.2.0

# Download WAS source code (checkout specified tag)
RUN git clone https://github.com/heywillow/willow-application-server /app
RUN git fetch --all --tags
RUN git checkout tags/$BACKEND_VERSION -b $BACKEND_VERSION

# Download WAS admin panel UI (at specified tag)
RUN wget https://github.com/heywillow/willow-application-server-ui/releases/download/$FRONTEND_VERSION/willow-application-server-ui.tar.gz
RUN mkdir -p /app/static/admin
RUN tar --directory=/app/static/admin/ --strip-components=1 -xzf willow-application-server-ui.tar.gz
RUN rm willow-application-server-ui.tar.gz

# Install Python dependencies for WAS
RUN pip install -r requirements.txt

# Expose ports
EXPOSE 8501
EXPOSE 8502

# Map volume for persistent data
VOLUME /config

# Copy and run entrypoint script
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
