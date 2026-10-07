# CavaLocal — Marketplace Platform 🍷

Marketplace intermediario de vinos para Caracas, Venezuela. Permite a los consumidores descubrir etiquetas nacionales e importadas, comparar precios en tiempo real entre múltiples licorerías, supermercados y bodegones caraqueños, y **reservar online con una seña del 20%** para retirar en tienda o solicitar entrega por delivery con cálculo de distancia real.

> **Identidad de marca:** Burdeos `#641E2E` · Dorado `#C2912B` · Crema `#F3ECDD`.

---

## 🏗️ Arquitectura del Repositorio

Este repositorio contiene la plataforma funcional completa (Frontend e-commerce + Backend API REST):

```
cavalocal-app/
├── web/              # Frontend oficial de e-commerce (HTML/CSS/JS ES Modules)
├── backend/          # API REST (NestJS 10, TypeScript, Prisma ORM, PostgreSQL)
└── docs/             # Especificaciones técnicas y de diseño
```

### 1. Frontend (`web/`)
* **Stack:** HTML5, CSS3, JavaScript Vanilla moderno (ES Modules nativos sin build step).
* **Mapas:** Integración con Leaflet + OpenStreetMap para visualización de sedes en Caracas y cálculo de distancia geodésica (Haversine).
* **Checkout:** Flujo de 4 pasos con selección de retiro o delivery (pin interactivo), desglose con descuento de primera compra (5%), seña (20%) y saldo (80%), pasarela simulada con validación Luhn y generación de factura imprimible.

### 2. Backend (`backend/`)
* **Stack:** NestJS 10, TypeScript, Prisma 5, PostgreSQL.
* **Autenticación:** JWT + Bcrypt + Google OAuth (Google Identity Services) + recuperación de contraseña con token temporal por correo.
* **Catálogo:** Filtros facetados por cepa, país, tipo y precio, ordenamiento y agregación de puntuaciones de crítica y reseñas de usuarios.
* **Reservas & Notificaciones:** Gestión del ciclo de vida de la reserva, Cron job horario para expiración de reservas inactivas y envío de facturas HTML automáticas vía Nodemailer (SMTP).

---

## 🚀 Inicio Rápido en Local

### 1. Requisitos Previos
* Node.js 20+ y `npm` (o `pnpm`)
* PostgreSQL activo (local o en la nube, ej. Neon / Supabase) con una base de datos `cavalocal`

### 2. Configurar y arrancar el Backend
```bash
cd backend
cp .env.example .env          # Ajusta DATABASE_URL y credenciales
npm install
npx prisma migrate deploy     # Aplica las migraciones
npx prisma generate
npm run prisma:seed           # Puebla datos de prueba (vinos y tiendas de Caracas)
npm run start:dev             # API activa en http://localhost:3001 (Swagger en /docs)
```

**Variables de entorno (`backend/.env`):**
* `DATABASE_URL`: Cadena de conexión PostgreSQL.
* `JWT_SECRET`: Secreto para firma de tokens JWT.
* `GOOGLE_CLIENT_ID`: (Opcional) Client ID de Google Cloud para inicio con Google.
* `MAIL_USER` / `MAIL_APP_PASSWORD`: (Opcional) Credenciales Gmail SMTP para envío de facturas.

### 3. Arrancar el Frontend
```bash
cd web
npx http-server . -p 8080
```
Abre tu navegador en `http://localhost:8080`.
*(La URL del backend se configura en `web/js/config.js`, por defecto apunta a `http://localhost:3001`)*.

---

## 🧪 Pruebas Unitarias

```bash
# Tests del frontend (node --test: validadores, carrusel, geolocalización, delivery)
cd web && npm test

# Tests del backend (Jest: autenticación, pagos, reservas, notificaciones)
cd backend && npm test
```

---

## 👥 Colaboradores
* **Aoshib** ([@Aoshi346](https://github.com/Aoshi346))
* **Eugenio Rijo** ([@EugenioRRijo](https://github.com/EugenioRRijo))
