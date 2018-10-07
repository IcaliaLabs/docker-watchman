# Facebook's Watchman for Docker Containers (Alpine)

[Watchman](https://facebook.github.io/watchman/) is a file watching service
developed by Facebook, and it's used by many projects (like Ember CLI) to watch
files for changes, triggering project rebuilds whenever a change is detected.

Instead of figuring out how to build watchman from source - or waiting for long
compilation times when building your Dockerfiles - you can use the "Multi-stage
Dockerfile" feature to copy the watchman executable directly from our image!

## Supported tags and respective Dockerfile links
- 4.9.0-alpine3.8, 4.9.0-alpine, 4.9-alpine3.8, 4.9-alpine, 4-alpine, 4, latest ([alpine/3.8/Dockerfile](https://github.com/IcaliaLabs/docker-watchman/master/blob/alpine/3.8/Dockerfile))
- 4.9.0-alpine3.4, 4.9-alpine3.4, 4-alpine3.4 ([alpine/3.4/Dockerfile](https://github.com/IcaliaLabs/docker-watchman/master/blob/alpine/3.4/Dockerfile))

## Recommended use case for Alpine-based images:

```
# 1: Start from whatever image you are using - this is a node app example:
FROM node:8-alpine

# 2: Install the packages required for watchman to work properly:
RUN apk add --no-cache libcrypto1.0 libgcc libstdc++

# 3: Copy the watchman executable binary, workdir and documentation (optional)
# directly from our image:
COPY --from=builder /usr/local/bin/watchman* /usr/local/bin/
COPY --from=builder /usr/local/var/run/watchman /usr/local/var/run/watchman
COPY --from=builder /usr/local/share/doc/watchman-4.9.0 /usr/local/share/doc/watchman-4.9.0

# 4: Continue with the rest of your Dockerfile:
COPY package.json /usr/src/
```

## References

### Similar projects:
- https://github.com/icalialabs/docker-wkhtmltopdf
