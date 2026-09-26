# Base Image 
FROM node:20-alpine AS builder

# Working directory of the application in container
WORKDIR /app

# Copying the files/copy all the files in
COPY . .

# Install the dependencies files -> stores to /node_modules
RUN npm install --legacy-peer-deps

# Frontend Build -> stores to /dist
RUN npm run build

# ---------------- Stage 1 ENDS ------------------------------

#----------------Stage 2 Starts ---------------------------

# Images -> light weight images for stage 2
#FROM node:20-slim AS runner
#FROM gcr.io/distroless/nodejs AS runner
FROM dhi.io/node:26 AS runner

# Working Directory
WORKDIR /app

# Copying from builder to this stage
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

COPY vite.preview.config.js ./vite.config.js

# To Run the application
CMD ["node","./node_modules/.bin/vite", "preview", "--host", "0.0.0.0", "--port", "4173"]
