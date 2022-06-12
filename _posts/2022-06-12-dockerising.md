# Dockerising

1. 간단한 node js 앱을 만든다

```jsx
// index.js

const app = require("express")();

app.get("/", (req, res) => res.json({ message: "DOcker is easy" }));

const port = process.env.PORT || 8080;

app.listen(port, () =>
  console.log(`app listening on http://localhost:${port}`)
);
```

2. Dockerfile 생성

docker file은 순서에 따라 작성

```docker
FROM node:12

WORKDIR /app

#의존성 설치
COPY package*.json ./

RUN npm install

# 모든 로컬 파일 복사, node_module는 .dockerignore에 장성해두기
COPY . .

# 환경변수
ENV PORT=8080

EXPOSE 8080

# 앱을 어떻게 구동하는가?
CMD [ 'node','index.js' ]
```

3. .dockerignore

```docker
node_modules
```

4. docker image 생성

```docker
docker build . -t <your username>/node-web-app
```

```docker
❯ docker build . -t jsndjsnd/dockerising
[+] Building 40.1s (11/11) FINISHED                                  
 => [internal] load build definition from Dockerfile            0.0s
 => => transferring dockerfile: 175B                            0.0s
 => [internal] load .dockerignore                               0.0s
 => => transferring context: 52B                                0.0s
 => [internal] load metadata for docker.io/library/node:12      3.1s
 => [auth] library/node:pull token for registry-1.docker.io     0.0s
 => [1/5] FROM docker.io/library/node:12@sha256:01627afeb110b  35.4s
 => => resolve docker.io/library/node:12@sha256:01627afeb110b3  0.0s
 => => sha256:01627afeb110b3054ba4a1405541ca095c8b 776B / 776B  0.0s
 => => sha256:212cfb481ff85e9344aea3297779fc75 7.70kB / 7.70kB  0.0s
 => => sha256:a41fbedfa4b1547a2b4fea33de48a 43.21MB / 43.21MB  12.5s
 => => sha256:6a94d3b1a91a1952461644c0005791 10.22MB / 10.22MB  5.1s
 => => sha256:4deb892a653993b3bc611a5099352812 2.21kB / 2.21kB  0.0s
 => => sha256:dad88c3c2eedd19cb4e591bedf0013f9 3.87MB / 3.87MB  0.6s
 => => sha256:67f90700f5859b0f6187aad3354827 47.74MB / 47.74MB  8.2s
 => => sha256:48979b276d7f5d48373126bb887 201.80MB / 201.80MB  29.2s
 => => sha256:9520e1bb28adfd56feabd558c21645d3 4.08kB / 4.08kB  8.4s
 => => sha256:b2423df6900250b9184f6296849fa 23.66MB / 23.66MB  12.2s
 => => sha256:8a1e736bfbf6b82984fdd71d0f2cbd1 2.34MB / 2.34MB  12.8s
 => => sha256:5e56b5666dd9246df190dc213033cf13dda 461B / 461B  12.8s
 => => extracting sha256:a41fbedfa4b1547a2b4fea33de48af745d66a  1.2s
 => => extracting sha256:6a94d3b1a91a1952461644c0005791faf07bf  0.3s
 => => extracting sha256:dad88c3c2eedd19cb4e591bedf0013f98bd12  0.1s
 => => extracting sha256:67f90700f5859b0f6187aad3354827bb2b243  1.4s
 => => extracting sha256:48979b276d7f5d48373126bb88786dfff3c3b  4.6s
 => => extracting sha256:9520e1bb28adfd56feabd558c21645d3a9e85  0.0s
 => => extracting sha256:b2423df6900250b9184f6296849fa00bb2135  0.8s
 => => extracting sha256:8a1e736bfbf6b82984fdd71d0f2cbd1f0aac2  0.1s
 => => extracting sha256:5e56b5666dd9246df190dc213033cf13dda6f  0.0s
 => [internal] load build context                               0.0s
 => => transferring context: 40.68kB                            0.0s
 => [2/5] WORKDIR /app                                          0.2s
 => [3/5] COPY package*.json ./                                 0.0s
 => [4/5] RUN npm install                                       1.2s
 => [5/5] COPY . .                                              0.0s
 => exporting to image                                          0.1s
 => => exporting layers                                         0.1s
 => => writing image sha256:60d056b156ad2f1cb9b1e97675db66ff1d  0.0s
 => => naming to docker.io/jsndjsnd/dockerising                 0.0s
```

5. docker hub 등에 배포
