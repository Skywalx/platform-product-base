FROM itzg/minecraft-server:java17-alpine

ENV EULA TRUE
ENV TYPE CUSTOM
ENV CUSTOM_SERVER ./purpur-1.17.1-1428.jar
# Skip runtime downloads - JAR is already baked into the image
ENV SKIP_SERVER_PATCHING true
ENV USE_AIKAR_FLAGS true
ENV JVM_DD_OPTS log4j.configurationFile=log4j-rce-fix.xml

# server properties
ENV VERSION 1.17
ENV MOTD "Skywalx product"
ENV DIFFICULTY peaceful
ENV SPAWN_MONSTERS false
ENV SPAWN_ANIMALS false
ENV SPAWN_NPCS false
ENV PVP false
ENV JVM_OPTS "-javaagent:slimeworldmanager-classmodifier-2.7.0-SNAPSHOT.jar"
ENV ALLOW_NETHER false
ENV ANNOUNCE_PLAYER_ACHIEVEMENTS false
ENV ENABLE_COMMAND_BLOCK false
ENV GENERATE_STRUCTURES false
ENV MODE adventure
ENV ONLINE_MODE false
ENV LEVEL world
ENV MAX_PLAYERS 6

ENV REPLACE_ENV_VARIABLES true
ENV ENFORCE_SECURE_PROFILE true

COPY --chown=1000:1000 --chmod=777 ./ /data
COPY --chown=1000:1000 --chmod=777 purpur-1.17.1-1428.jar /data/purpur.jar

# Pre-run Paperclip during build to download and patch vanilla JAR
WORKDIR /data
RUN timeout 120 java -jar purpur.jar || true
RUN rm -f /data/eula.txt /data/server.properties /data/logs/* 2>/dev/null || true
RUN chown -R 1000:1000 /data

EXPOSE 25565
EXPOSE 80
EXPOSE 443