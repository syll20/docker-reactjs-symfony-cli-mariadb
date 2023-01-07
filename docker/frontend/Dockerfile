# https://registry.hub.docker.com/_/node

ARG NODEJS
FROM node:${NODEJS} 

ENV PATH /app/node_modules/.bin:$PATH

WORKDIR /app
COPY ./codebase/package.j[son] /app
COPY ./codebase/package-lock.j[son] /app
COPY ./codebase /app

ARG NPM
RUN npm install 

#ENV TERM xterm-256color
RUN echo PS1="'"'\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\ \033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '"'" >> ~/.bashrc
RUN echo alias ls="'"'ls --color=auto'"'" >> ~/.bashrc

ARG APP_FRONTEND_PORT}
EXPOSE ${APP_FRONTEND_PORT}
CMD ["npm", "start"]
