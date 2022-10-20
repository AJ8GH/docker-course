FROM node
WORKDIR /app
COPY package.json .
COPY package-lock.json .
RUN npm install
RUN npm install react-scripts@3.4.1 -g
COPY . .
EXPOSE 3000
ENV BE=backend
CMD [ "npm", "start" ]
