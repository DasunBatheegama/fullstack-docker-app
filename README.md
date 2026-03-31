# Fullstack Docker App

This repository demonstrates how to dockerize a React/Vite frontend and a Node.js/Express backend separately for development.

## Project Structure

- `frontend`: React + Vite app (runs on container port 5173)
- `Backend`: Node.js + Express API (runs on container port 5000)

## Prerequisites

- Docker Desktop installed and running
- Open a terminal at repository root

## 1) Build Docker Images

From repository root:

- `docker build -t react-app ./frontend`
- `docker build -t app ./Backend`

## 2) Frontend Development with Docker (React/Vite)

Run from `frontend` folder:

- `docker run --name react-container --rm -p 3000:5173 -v /app/node_modules -v ${PWD}:/app -e CHOKIDAR_USEPOLLING=true react-app`

What this does:

- `-p 3000:5173`: Maps host port 3000 to Vite inside container on 5173
- `-e CHOKIDAR_USEPOLLING=true`: Enables file polling so HMR/live-reload works through mounted volumes
- `-v /app/node_modules`: Keeps container dependencies isolated from host overwrite
- `-v ${PWD}:/app`: Mounts your local source into the container for live development

Open frontend at:

- `http://localhost:3000`

## 3) Backend Development with Docker (Node.js/Express)

Run from `Backend` folder:

- `docker run --name app-container --rm -p 5000:5000 -v /app/node_modules -v ${PWD}:/app app`

What this does:

- `-p 5000:5000`: Exposes backend API on localhost:5000
- `-v /app/node_modules`: Protects container node_modules from host overwrite
- `-v ${PWD}:/app`: Mounts backend source for live updates

Backend endpoints:

- `http://localhost:5000/posts`
- `http://localhost:5000/post/1`

## 4) Simple Backend Run (No Live Reload)

Useful for quick testing without volume mounts:

- `docker run --rm --name docker-server-container -p 5000:5000 app`

## 5) Deep Cleaning Docker Space

Use when Docker consumes too much disk space:

- `docker system prune -a`

This removes:

- Stopped containers
- Unused networks
- Build cache
- All unused images

## Notes

- Keep frontend and backend in separate terminals when developing both together.
- Since `--rm` is used, containers are removed automatically when stopped.
- If you run commands from repository root instead of each app folder, update mounts to:
  - Frontend: `-v ${PWD}/frontend:/app`
  - Backend: `-v ${PWD}/Backend:/app`