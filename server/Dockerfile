# Multi-stage build for size optimization
FROM alpine:3.19 AS optimizer

# Remove unnecessary files and optimize plugin configs
WORKDIR /tmp/data
COPY . .

# Remove development/debug files and optimize configs
RUN find . -name "*.log" -delete && \
    find . -name "*.tmp" -delete && \
    find . -name ".DS_Store" -delete && \
    find . -name "*.backup" -delete && \
    find . -name "lastupdate" -delete && \
    rm -rf world/session.lock world/uid.dat 2>/dev/null || true

# Final optimized image
FROM itzg/minecraft-server:java17-alpine

# Single ENV block for better layer caching
ENV EULA=TRUE \
    TYPE=CUSTOM \
    CUSTOM_SERVER=./purpur-1.17.1-1428.jar \
    USE_AIKAR_FLAGS=true \
    JVM_DD_OPTS="log4j.configurationFile=log4j-rce-fix.xml" \
    VERSION=1.17 \
    MOTD="Skywalx product" \
    DIFFICULTY=peaceful \
    SPAWN_MONSTERS=false \
    SPAWN_ANIMALS=false \
    SPAWN_NPCS=false \
    PVP=false \
    JVM_OPTS="-javaagent:slimeworldmanager-classmodifier-2.7.0-SNAPSHOT.jar -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1" \
    ALLOW_NETHER=false \
    ANNOUNCE_PLAYER_ACHIEVEMENTS=false \
    ENABLE_COMMAND_BLOCK=false \
    GENERATE_STRUCTURES=false \
    MODE=adventure \
    ONLINE_MODE=false \
    LEVEL=world \
    MAX_PLAYERS=6 \
    REPLACE_ENV_VARIABLES=true \
    ENFORCE_SECURE_PROFILE=true \
    MEMORY=2G

# Copy optimized data with proper permissions
COPY --from=optimizer --chown=1000:1000 --chmod=755 /tmp/data /data

# Only expose necessary port
EXPOSE 25565