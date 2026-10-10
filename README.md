# CICD

## Exemple d'utilisation pour github

```
name: Deploy

on:
  push:
    tags:
      - 'v*'

jobs:
  deploy:
    uses: redyd/.github/.github/workflows/deploy.yml@main
    with:
      image_name: projet/<nom de l'image>
    secrets:
      REGISTRY_USER: ${{ secrets.REGISTRY_USER }}
      REGISTRY_PASSWORD: ${{ secrets.REGISTRY_PASSWORD }}
```

## Exemple d'utilisation pour gitlab

```
include:
  - remote: 'https://raw.githubusercontent.com/redyd/.github/main/gitlab/deploy.yml'
    inputs:
      image_name: projet/<nom de l'image>

workflow:
  rules:
    - if: $CI_COMMIT_TAG =~ /^v/
```

**Veuillez à bien ajouter les variables dans le repo**

**Ne pas oubliez d'ajouter dans le docker-compose le networks proxy**:
```
services:
  app:
    image: registry.redyd.dev/<nom de l'image>:latest
    container_name: <nom du container>
    restart: unless-stopped
    ports:
      - "3000:3000"
    networks:
      - proxy

networks:
  proxy:
    external: true
```
