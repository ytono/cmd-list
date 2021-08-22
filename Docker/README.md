# Docker

```
docker run hello-world

docker images

docker ps

docker ps -a
```

```
# ビルド
mkdir test && cd test

cat > Dockerfile <<EOF
# 上位イメージとして正式な Node ランタイムを使用します
FROM node:6
# コンテナの作業ディレクトリを /app に設定します
WORKDIR /app
# 現行ディレクトリの内容を /app のコンテナにコピーします
ADD . /app
# コンテナのポート 80 で外部からアクセスできるようにします
EXPOSE 80
# コンテナの起動時に node を使用して app.js を実行します
CMD ["node", "app.js"]
EOF

docker build -t node-app:0.1 .

docker run -p 4000:80 --name my-app node-app:0.1

curl http://localhost:4000

docker stop my-app && docker rm my-app
```

```
# デバッグ
docker logs [コンテナ ID]

docker logs -f [コンテナ ID]

docker exec -it [コンテナ ID] bash

docker inspect [コンテナ ID]

docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [コンテナ ID]
```

```
# 公開
docker tag node-app:0.2 gcr.io/${PROJECT_ID}/node-app:0.2

docker images

docker push gcr.io/$PROJECT_ID/node-app:0.2

docker stop $(docker ps -q)
docker rm $(docker ps -aq)

docker rmi node-app:0.2 gcr.io/$PROJECT_ID/node-app node-app:0.1
docker rmi node:6
docker rmi $(docker images -aq) # remove remaining images
docker images

docker pull gcr.io/$PROJECT_ID/node-app:0.2
docker run -p 4000:80 -d gcr.io/$PROJECT_ID/node-app:0.2
curl http://localhost:4000
```

