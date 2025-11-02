FROM node:20-alpine AS build

ARG DATABASE_DIRECT_URL
ARG DATABASE_URL

WORKDIR /calcom

# Install necessary packages for ARM64 compatibility
RUN apk add --no-cache libc6-compat
RUN set -eux;
# Check if libssl.so.3 exists before creating symlink
RUN if [ -f /usr/lib/libssl.so.3 ] && [ ! -f /lib/libssl.so.3 ]; then ln -s /usr/lib/libssl.so.3 /lib/libssl.so.3; fi

ENV NODE_ENV="production"
ENV NODE_OPTIONS="--max-old-space-size=8192"
ENV DATABASE_DIRECT_URL=${DATABASE_DIRECT_URL}
ENV DATABASE_URL=${DATABASE_URL}

COPY . .

RUN yarn install
RUN yarn workspace @calcom/api-v2 run generate-schemas
RUN yarn workspace @calcom/platform-libraries run build
RUN yarn workspace @calcom/api-v2 run build

EXPOSE 80

CMD [ "yarn", "workspace", "@calcom/api-v2", "start:prod"]
