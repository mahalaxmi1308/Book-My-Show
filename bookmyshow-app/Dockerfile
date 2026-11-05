# Node.js base image vadukuntu Dockerfile create cheddam
FROM node:23-alpine

# Container lo working directory set cheyali
WORKDIR /app

# Dependencies install cheyadaniki package.json files copy cheyali
COPY package*.json ./

# Node modules install cheyali
RUN npm install

# Migata source code motham copy cheyali
COPY . .

# App run ayye port (meeru k8s deployment lo 8080 vadaru kabatti 8080 ivvachu)
EXPOSE 8080

# Application start chese command
CMD ["npm", "start"]
