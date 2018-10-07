################################################################################
# Stage 1: Runtime
FROM alpine:3.8 AS runtime

RUN apk add --no-cache \
  libcrypto1.0 \
  libgcc \
  libstdc++

################################################################################
# Stage 2: Builder
FROM runtime AS builder

# Step 1: Install the build dependencies:
RUN apk add --no-cache --update ca-certificates openssl

RUN apk add --no-cache \
 automake \
 autoconf \
 bash \
 build-base \
 libtool \
 linux-headers \
 openssl-dev \
 python-dev

# Step 2: Set environment variables:
ENV WATCHMAN_VERSION=4.9.0 \
    WATCHMAN_SHA256=1f6402dc70b1d056fffc3748f2fdcecff730d8843bb6936de395b3443ce05322

# Step 3: Download watchman source code:
RUN cd /tmp \
 && wget -O watchman.tar.gz "https://github.com/facebook/watchman/archive/v${WATCHMAN_VERSION}.tar.gz" \
 && echo "$WATCHMAN_SHA256 *watchman.tar.gz" | sha256sum -c - \
 && tar -xz -f watchman.tar.gz -C /tmp/ \
 && rm -rf watchman.tar.gz

# Step 4: Build watchman from source:
RUN cd /tmp/watchman-${WATCHMAN_VERSION} \
 && ./autogen.sh \
 && ./configure \
 && make \
 && make install \
 && cd $HOME \
 && rm -rf /tmp/*

################################################################################
# Stage 3: Release
FROM runtime AS release

# Step 1-3: Copy compiled executable, documentation and runtime directories:
COPY --from=builder /usr/local/bin/watchman* /usr/local/bin/
COPY --from=builder /usr/local/share/doc/watchman-4.9.0 /usr/local/share/doc/watchman-4.9.0
COPY --from=builder /usr/local/var/run/watchman /usr/local/var/run/watchman
