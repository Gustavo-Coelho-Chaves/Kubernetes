# Etapa de build
FROM node:18-alpine AS build

WORKDIR /app

# Instala dependências necessárias para compilar pacotes nativos
RUN apk add --no-cache python3 make g++

# Copia apenas os arquivos de dependências primeiro (para cache de camadas)
COPY package.json package-lock.json ./

# Executa instalação de dependências
RUN npm install

# Copia os demais arquivos do projeto
COPY . .

# Define variável de ambiente para compatibilidade de OpenSSL (usado em alguns projetos React/Vite)
ENV NODE_OPTIONS=--openssl-legacy-provider

# Corrigido: "rum" para "run"
RUN npm run build

# Etapa final: imagem leve com Nginx
FROM nginx:alpine

# Copia a build gerada para o diretório padrão do Nginx
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
