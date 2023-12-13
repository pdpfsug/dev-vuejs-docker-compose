FROM node:20-alpine AS development

RUN mkdir -p /project
WORKDIR /project

COPY . .

RUN corepack enable && corepack prepare pnpm@latest --activate
ENV PNPM_HOME=/usr/local/bin/
ENV SHELL=/bin/sh
ENV ENV=/etc/profile
RUN pnpm setup 
RUN source /etc/profile
RUN pnpm add -g @vue/cli
RUN pnpm install
ENV HOST=0.0.0.0
CMD ["pnpm", "run", "serve"]

FROM development as dev-envs
RUN apk update && apk add git

RUN addgroup -S docker && adduser -S --shell /bin/bash --ingroup docker vscode

# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
CMD ["pnpm", "run", "serve"]

