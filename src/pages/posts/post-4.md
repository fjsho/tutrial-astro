---
layout: ../../layouts/MarkdownPostLayout.astro
title: "DockerでNode.js開発環境を構築する"
author: "Tech Blog"
description: "Dockerを使ってNode.jsの開発環境を構築し、チーム全体で統一された環境を実現する方法を紹介します"
image:
  url: "https://docs.astro.build/default-og-image.png"
  alt: "Dockerのイメージ"
pubDate: 2024-11-05
tags: ["Docker", "Node.js", "開発環境"]
---

Dockerを使うことで、開発環境をコンテナ化し、チームメンバー全員が同じ環境で開発できるようになります。Node.jsプロジェクトでのDocker活用方法を紹介します。

## Dockerfileの作成

まず、プロジェクトのルートに`Dockerfile`を作成します。

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

## docker-compose.ymlの設定

開発環境では、docker-composeを使うと便利です。

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```

## 実行方法

以下のコマンドで開発環境を起動できます。

```bash
docker-compose up
```

ホットリロードも有効なので、ファイルを編集すると自動的に反映されます。

## まとめ

Dockerを活用することで、「私の環境では動く」問題を解決し、チーム全体で統一された開発環境を実現できます。
