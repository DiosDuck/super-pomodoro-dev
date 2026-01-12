FROM node:20-alpine

RUN npm install -g @angular/cli

WORKDIR /app

CMD ["sh", "-c", "npm ci && npx ng build --watch --poll 200"]
