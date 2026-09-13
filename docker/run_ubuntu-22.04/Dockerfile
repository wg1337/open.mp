FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
ARG TARGET_CONTAINER_PLATFORM

# Base layer
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        wget \
        tzdata \
        openssl \
        python3 \
        libssl3 && \
    rm -rf /var/lib/apt/lists/* && \
    useradd -m user

# Arch-specific layer
RUN case "$TARGET_CONTAINER_PLATFORM" in \
        linux/amd64) \
            dpkg --add-architecture i386 && \
            apt-get update && \
            apt-get install -y --no-install-recommends \
                libstdc++6:i386 \
                libssl3:i386 \
            ;; \
        linux/arm|linux/arm64) \
            apt-get update && \
            apt-get install -y --no-install-recommends \
                libstdc++6 \
                libatomic1 \
            ;; \
        *) \
            echo "Unsupported TARGET_CONTAINER_PLATFORM: $TARGET_CONTAINER_PLATFORM" >&2 \
            exit 1 \
            ;; \
    esac && \
    rm -rf /var/lib/apt/lists/*

USER user
WORKDIR /home/user

COPY open.mp /opt/open.mp
CMD /opt/open.mp/omp-server
