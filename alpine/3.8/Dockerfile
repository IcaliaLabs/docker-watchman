# Stage I: Runtime =============================================================

# Step 1: Begin with Alpine 3.8:
FROM alpine:3.8 AS runtime

# Step 2: Install watchman runtime dependencies:
RUN apk add --no-cache libcrypto1.0 libgcc libstdc++

# Stage II: Builder ============================================================

# Step 3: Begin from runtime stage image:
FROM runtime AS builder

# Step 4: Install the watchman build dependencies' dependencies :)
RUN apk add --no-cache --update ca-certificates openssl

# Step 5: Install the watchman build dependencies:
RUN apk add --no-cache \
 automake \
 autoconf \
 bash \
 build-base \
 libtool \
 linux-headers \
 openssl-dev \
 python-dev

# Step 6: Set environment variables:
ENV WATCHMAN_VERSION=4.9.0 \
    WATCHMAN_SHA256=1f6402dc70b1d056fffc3748f2fdcecff730d8843bb6936de395b3443ce05322

# Step 7: Download watchman source code:
RUN cd /tmp \
 && wget -O watchman.tar.gz "https://github.com/facebook/watchman/archive/v${WATCHMAN_VERSION}.tar.gz" \
 && echo "$WATCHMAN_SHA256 *watchman.tar.gz" | sha256sum -c - \
 && tar -xz -f watchman.tar.gz -C /tmp/ \
 && rm -rf watchman.tar.gz

# Step 8: Build watchman from source:
RUN cd /tmp/watchman-${WATCHMAN_VERSION} \
 && ./autogen.sh \
 && ./configure \
 && make \
 && make install \
 && cd $HOME \
 && rm -rf /tmp/*

# Stage III: Release ===========================================================

# Step 9: Begin with the runtime stage image:
FROM runtime AS release

# Step 10: Copy the compiled executable:
COPY --from=builder /usr/local/bin/watchman* /usr/local/bin/

# Step 11: Copy the documentation:
COPY --from=builder /usr/local/share/doc/watchman-4.9.0 /usr/local/share/doc/watchman-4.9.0

# Step 12: Copy the runtime directories:
COPY --from=builder /usr/local/var/run/watchman /usr/local/var/run/watchman
