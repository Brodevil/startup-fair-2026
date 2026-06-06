# syntax=docker/dockerfile:1.7

FROM node:22-alpine AS build

WORKDIR /app

COPY . .

ARG VITE_API_URL=/api
ARG REACT_APP_API_URL=/api
ARG NEXT_PUBLIC_API_URL=/api
ENV VITE_API_URL=${VITE_API_URL}
ENV REACT_APP_API_URL=${REACT_APP_API_URL}
ENV NEXT_PUBLIC_API_URL=${NEXT_PUBLIC_API_URL}

RUN if [ -f yarn.lock ]; then yarn install --frozen-lockfile; \
    elif [ -f package-lock.json ]; then npm ci; \
    else npm install; fi

RUN if [ -f yarn.lock ]; then yarn build; \
    else npm run build; fi

RUN mkdir -p /out && \
    if [ -d dist ]; then cp -R dist/. /out/; \
    elif [ -d build ]; then cp -R build/. /out/; \
    elif [ -d out ]; then cp -R out/. /out/; \
    else echo "No frontend build output found" && exit 1; fi

FROM nginx:1.27-alpine AS runtime

COPY ./default.conf /etc/nginx/conf.d/default.conf

COPY --from=build /out/ /usr/share/nginx/html/

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
