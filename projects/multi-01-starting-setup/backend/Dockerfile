FROM node
WORKDIR /app
COPY package.json .
COPY package-lock.json .
RUN npm install
COPY . .
EXPOSE 80
ENV DB=multi-01-starting-setup-mongo-1
CMD [ "npm", "start" ]
