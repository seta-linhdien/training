FROM node:latest
WORKDIR /Users/linhdv/Download/homework
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [ "npm", "start" ]