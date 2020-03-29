# Facebook's Watchman for Docker Containers

[Watchman](https://facebook.github.io/watchman/) is a file watching service
developed by Facebook, and it's used by many projects (like Ember CLI) to watch
files for changes, triggering project rebuilds whenever a change is detected.

Instead of figuring out how to build watchman from source - or waiting for long
compilation times when building your Dockerfiles - you can use the "Multi-stage
Dockerfile" feature to copy the watchman executable directly from our image!

## Supported tags and respective Dockerfile links
- [4.9.0-alpine3.8, 4.9.0-alpine, 4.9-alpine3.8, 4.9-alpine, 4-alpine, 4, latest](https://github.com/IcaliaLabs/docker-watchman/master/blob/alpine/3.8/Dockerfile)
- [4.9.0-alpine3.4, 4.9-alpine3.4, 4-alpine3.4](https://github.com/IcaliaLabs/docker-watchman/master/blob/alpine/3.4/Dockerfile)
- [debian, buster](https://github.com/IcaliaLabs/docker-watchman/master/blob/debian/buster/Dockerfile)

## Recommended usage on Alpine-based images:

```
# Start from whatever image you are using - this is a node app example:
FROM node:8-alpine

# Install the packages required for watchman to work properly:
RUN apk add --no-cache libcrypto1.0 libgcc libstdc++

# Copy the watchman executable binary directly from our image:
COPY --from=icalialabs/watchman:4-alpine3.4 /usr/local/bin/watchman* /usr/local/bin/

# Create the watchman STATEDIR directory:
RUN mkdir -p /usr/local/var/run/watchman \
 && touch /usr/local/var/run/watchman/.not-empty

# (Optional) Copy the compiled watchman documentation:
COPY --from=icalialabs/watchman:4-alpine3.4 /usr/local/share/doc/watchman* /usr/local/share/doc/

# Continue with the rest of your Dockerfile...
```

## References

### Similar projects:
- https://github.com/icalialabs/docker-wkhtmltopdf
