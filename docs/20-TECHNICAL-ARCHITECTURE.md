# Architecture Technique Complète

**Stack technologique détaillée pour l'ERP IUT Douala**

---

## Table des Matières

1. [Stack Globale](#1-stack-globale)
2. [Architecture Monorepo](#2-architecture-monorepo)
3. [Frontend (Next.js)](#3-frontend-nextjs)
4. [Backend (NestJS)](#4-backend-nestjs)
5. [Base de Données](#5-base-de-données)
6. [Schéma Prisma Complet](#6-schéma-prisma-complet)
7. [Infrastructure & Déploiement](#7-infrastructure--déploiement)

---

## 1. Stack Globale

### 1.1 Vue d'Ensemble

```
┌─────────────────────────────────────────────────────────────────┐
│                      STACK TECHNIQUE                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Frontend (apps/web)                                            │
│  ├── Next.js 14 (App Router)                                   │
│  ├── TypeScript 5+                                              │
│  ├── TailwindCSS 3                                              │
│  ├── shadcn/ui (Radix UI)                                      │
│  └── React Hook Form + Zod                                      │
│                                                                  │
│  Backend (apps/api)                                             │
│  ├── NestJS 10+                                                 │
│  ├── TypeScript 5+                                              │
│  ├── Prisma ORM                                                 │
│  ├── Passport.js (JWT)                                          │
│  └── class-validator                                            │
│                                                                  │
│  Database                                                        │
│  ├── PostgreSQL 15+                                             │
│  ├── Redis 7+                                                   │
│  └── MinIO / S3                                                 │
│                                                                  │
│  Monorepo                                                        │
│  ├── Turborepo                                                  │
│  ├── pnpm workspaces                                            │
│  └── Shared packages                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2. Architecture Monorepo

### 2.1 Structure Complète

```
erp-iut-douala/
├── apps/
│   ├── web/                          # Next.js 14 App
│   │   ├── app/
│   │   │   ├── (auth)/
│   │   │   │   ├── login/
│   │   │   │   └── register/
│   │   │   ├── (dashboard)/
│   │   │   │   ├── admin/
│   │   │   │   ├── scolarite/
│   │   │   │   └── finance/
│   │   │   ├── (student)/
│   │   │   │   ├── dashboard/
│   │   │   │   ├── grades/
│   │   │   │   ├── payments/
│   │   │   │   └── documents/
│   │   │   ├── (teacher)/
│   │   │   │   ├── dashboard/
│   │   │   │   ├── courses/
│   │   │   │   ├── grades/
│   │   │   │   └── attendance/
│   │   │   └── api/                  # API Routes (si besoin)
│   │   ├── components/
│   │   │   ├── ui/                   # shadcn/ui components
│   │   │   ├── forms/
│   │   │   ├── tables/
│   │   │   ├── charts/
│   │   │   └── layouts/
│   │   ├── lib/
│   │   │   ├── api-client.ts
│   │   │   ├── auth.ts
│   │   │   └── utils.ts
│   │   ├── public/
│   │   ├── styles/
│   │   ├── package.json
│   │   ├── next.config.js
│   │   └── tsconfig.json
│   │
│   ├── api/                          # NestJS Backend
│   │   ├── src/
│   │   │   ├── modules/
│   │   │   │   ├── auth/
│   │   │   │   │   ├── auth.controller.ts
│   │   │   │   │   ├── auth.service.ts
│   │   │   │   │   ├── auth.module.ts
│   │   │   │   │   ├── strategies/
│   │   │   │   │   └── guards/
│   │   │   │   ├── students/
│   │   │   │   │   ├── students.controller.ts
│   │   │   │   │   ├── students.service.ts
│   │   │   │   │   ├── students.module.ts
│   │   │   │   │   └── dto/
│   │   │   │   ├── teachers/
│   │   │   │   ├── courses/
│   │   │   │   ├── grades/
│   │   │   │   ├── attendance/
│   │   │   │   ├── finance/
│   │   │   │   ├── documents/
│   │   │   │   ├── notifications/
│   │   │   │   └── reporting/
│   │   │   ├── common/
│   │   │   │   ├── decorators/
│   │   │   │   ├── filters/
│   │   │   │   ├── guards/
│   │   │   │   ├── interceptors/
│   │   │   │   ├── pipes/
│   │   │   │   └── utils/
│   │   │   ├── config/
│   │   │   ├── app.module.ts
│   │   │   └── main.ts
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   ├── migrations/
│   │   │   └── seed.ts
│   │   ├── test/
│   │   ├── package.json
│   │   ├── nest-cli.json
│   │   └── tsconfig.json
│   │
│   └── mobile/                       # React Native (optionnel)
│       ├── src/
│       ├── android/
│       ├── ios/
│       └── package.json
│
├── packages/
│   ├── database/                     # Prisma client partagé
│   │   ├── prisma/
│   │   │   └── schema.prisma
│   │   ├── src/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── types/                        # Types partagés
│   │   ├── src/
│   │   │   ├── auth.ts
│   │   │   ├── student.ts
│   │   │   ├── teacher.ts
│   │   │   ├── course.ts
│   │   │   ├── grade.ts
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── ui/                           # Composants UI partagés
│   │   ├── src/
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── table.tsx
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── utils/                        # Utilitaires partagés
│   │   ├── src/
│   │   │   ├── grades/
│   │   │   │   ├── calculate-average.ts
│   │   │   │   ├── calculate-credits.ts
│   │   │   │   └── validate-ue.ts
│   │   │   ├── dates/
│   │   │   ├── formatting/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── config/                       # Configuration partagée
│       ├── eslint-config-custom/
│       ├── tsconfig/
│       └── tailwind-config/
│
├── docs/                             # Documentation
│   ├── 00-INDEX.md
│   ├── 01-OVERVIEW.md
│   ├── ...
│   └── images/
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── deploy.yml
│
├── docker/
│   ├── Dockerfile.web
│   ├── Dockerfile.api
│   └── docker-compose.yml
│
├── .gitignore
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
├── README.md
└── LICENSE
```

### 2.2 Configuration Turborepo

```json
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": [
    "**/.env.*local"
  ],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "!.next/cache/**", "dist/**"]
    },
    "lint": {
      "dependsOn": ["^lint"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": ["coverage/**"]
    },
    "db:migrate": {
      "cache": false
    },
    "db:push": {
      "cache": false
    },
    "db:seed": {
      "cache": false
    }
  }
}
```

```yaml
# pnpm-workspace.yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

```json
// package.json (root)
{
  "name": "erp-iut-douala",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "lint": "turbo run lint",
    "test": "turbo run test",
    "db:migrate": "turbo run db:migrate",
    "db:push": "turbo run db:push",
    "db:seed": "turbo run db:seed",
    "clean": "turbo run clean && rm -rf node_modules"
  },
  "devDependencies": {
    "turbo": "latest",
    "prettier": "latest",
    "eslint": "latest"
  },
  "packageManager": "pnpm@8.15.0",
  "engines": {
    "node": ">=18.0.0",
    "pnpm": ">=8.0.0"
  }
}
```

---

## 3. Frontend (Next.js)

### 3.1 Configuration

```typescript
// apps/web/next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  transpilePackages: ['@erp/ui', '@erp/types', '@erp/utils'],
  
  images: {
    domains: ['storage.erp.iut.cm', 'localhost'],
    formats: ['image/webp', 'image/avif']
  },
  
  env: {
    NEXT_PUBLIC_API_URL: process.env.NEXT_PUBLIC_API_URL,
  },
  
  experimental: {
    serverActions: true,
  },
}

module.exports = nextConfig
```

### 3.2 Client API

```typescript
// apps/web/lib/api-client.ts

import axios from 'axios';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Intercepteur pour ajouter le token
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Intercepteur pour refresh token
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refreshToken');
        const { data } = await axios.post(
          `${process.env.NEXT_PUBLIC_API_URL}/auth/refresh`,
          { refreshToken }
        );
        
        localStorage.setItem('accessToken', data.accessToken);
        originalRequest.headers.Authorization = `Bearer ${data.accessToken}`;
        
        return apiClient(originalRequest);
      } catch (refreshError) {
        // Redirect to login
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }
    
    return Promise.reject(error);
  }
);

export default apiClient;
```

### 3.3 UI Components (shadcn/ui)

```bash
# Installation
npx shadcn-ui@latest init

# Ajouter composants
npx shadcn-ui@latest add button
npx shadcn-ui@latest add input
npx shadcn-ui@latest add table
npx shadcn-ui@latest add dialog
npx shadcn-ui@latest add select
npx shadcn-ui@latest add toast
```

---

## 4. Backend (NestJS)

### 4.1 Configuration

```typescript
// apps/api/src/main.ts

import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    cors: {
      origin: process.env.FRONTEND_URL,
      credentials: true,
    },
  });
  
  // Global prefix
  app.setGlobalPrefix('api');
  
  // Validation pipe
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      transform: true,
      forbidNonWhitelisted: true,
    })
  );
  
  // Swagger documentation
  const config = new DocumentBuilder()
    .setTitle('ERP IUT Douala API')
    .setDescription('API Documentation')
    .setVersion('1.0')
    .addBearerAuth()
    .build();
  const document = SwaggerModule.createDocument(app, config);
  SwaggerModule.setup('api/docs', app, document);
  
  await app.listen(3001);
  console.log(`🚀 API running on http://localhost:3001`);
}

bootstrap();
```

### 4.2 Module Example

```typescript
// apps/api/src/modules/students/students.module.ts

import { Module } from '@nestjs/common';
import { StudentsController } from './students.controller';
import { StudentsService } from './students.service';
import { PrismaModule } from '../prisma/prisma.module';

@Module({
  imports: [PrismaModule],
  controllers: [StudentsController],
  providers: [StudentsService],
  exports: [StudentsService],
})
export class StudentsModule {}
```

```typescript
// apps/api/src/modules/students/students.controller.ts

import {
  Controller,
  Get,
  Post,
  Body,
  Param,
  Put,
  Delete,
  Query,
  UseGuards,
} from '@nestjs/common';
import { ApiTags, ApiBearerAuth, ApiOperation } from '@nestjs/swagger';
import { StudentsService } from './students.service';
import { CreateStudentDto, UpdateStudentDto } from './dto';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';
import { RolesGuard } from '../auth/guards/roles.guard';
import { Roles } from '../auth/decorators/roles.decorator';

@ApiTags('students')
@ApiBearerAuth()
@Controller('students')
@UseGuards(JwtAuthGuard, RolesGuard)
export class StudentsController {
  constructor(private readonly studentsService: StudentsService) {}
  
  @Post()
  @Roles('ADMIN', 'SCOLARITE')
  @ApiOperation({ summary: 'Create a new student' })
  create(@Body() createStudentDto: CreateStudentDto) {
    return this.studentsService.create(createStudentDto);
  }
  
  @Get()
  @Roles('ADMIN', 'SCOLARITE', 'TEACHER')
  @ApiOperation({ summary: 'Get all students' })
  findAll(
    @Query('page') page: number = 1,
    @Query('limit') limit: number = 10,
    @Query('search') search?: string,
  ) {
    return this.studentsService.findAll({ page, limit, search });
  }
  
  @Get(':id')
  @ApiOperation({ summary: 'Get a student by ID' })
  findOne(@Param('id') id: string) {
    return this.studentsService.findOne(id);
  }
  
  @Put(':id')
  @Roles('ADMIN', 'SCOLARITE')
  @ApiOperation({ summary: 'Update a student' })
  update(
    @Param('id') id: string,
    @Body() updateStudentDto: UpdateStudentDto,
  ) {
    return this.studentsService.update(id, updateStudentDto);
  }
  
  @Delete(':id')
  @Roles('ADMIN')
  @ApiOperation({ summary: 'Delete a student' })
  remove(@Param('id') id: string) {
    return this.studentsService.remove(id);
  }
}
```

```typescript
// apps/api/src/modules/students/students.service.ts

import { Injectable, NotFoundException } from '@nestjs/common';
import { PrismaService } from '../prisma/prisma.service';
import { CreateStudentDto, UpdateStudentDto } from './dto';

@Injectable()
export class StudentsService {
  constructor(private prisma: PrismaService) {}
  
  async create(createStudentDto: CreateStudentDto) {
    return this.prisma.student.create({
      data: createStudentDto,
    });
  }
  
  async findAll(params: { page: number; limit: number; search?: string }) {
    const { page, limit, search } = params;
    const skip = (page - 1) * limit;
    
    const where = search
      ? {
          OR: [
            { firstName: { contains: search, mode: 'insensitive' } },
            { lastName: { contains: search, mode: 'insensitive' } },
            { matricule: { contains: search, mode: 'insensitive' } },
          ],
        }
      : {};
    
    const [students, total] = await Promise.all([
      this.prisma.student.findMany({
        where,
        skip,
        take: limit,
        orderBy: { createdAt: 'desc' },
      }),
      this.prisma.student.count({ where }),
    ]);
    
    return {
      data: students,
      meta: {
        page,
        limit,
        total,
        totalPages: Math.ceil(total / limit),
      },
    };
  }
  
  async findOne(id: string) {
    const student = await this.prisma.student.findUnique({
      where: { id },
      include: {
        enrollments: {
          include: { course: true }
        },
        grades: true,
        payments: true,
      },
    });
    
    if (!student) {
      throw new NotFoundException(`Student with ID ${id} not found`);
    }
    
    return student;
  }
  
  async update(id: string, updateStudentDto: UpdateStudentDto) {
    await this.findOne(id); // Check exists
    
    return this.prisma.student.update({
      where: { id },
      data: updateStudentDto,
    });
  }
  
  async remove(id: string) {
    await this.findOne(id); // Check exists
    
    return this.prisma.student.delete({
      where: { id },
    });
  }
}
```

---

## 5. Base de Données

### 5.1 PostgreSQL Configuration

```yaml
# docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    container_name: erp-postgres
    environment:
      POSTGRES_DB: erp_iut
      POSTGRES_USER: erp_user
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - '5432:5432'
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped
  
  redis:
    image: redis:7-alpine
    container_name: erp-redis
    ports:
      - '6379:6379'
    volumes:
      - redis_data:/data
    restart: unless-stopped
  
  minio:
    image: minio/minio
    container_name: erp-minio
    environment:
      MINIO_ROOT_USER: ${MINIO_ROOT_USER}
      MINIO_ROOT_PASSWORD: ${MINIO_ROOT_PASSWORD}
    ports:
      - '9000:9000'
      - '9001:9001'
    volumes:
      - minio_data:/data
    command: server /data --console-address ":9001"
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  minio_data:
```

### 5.2 Prisma Setup

```prisma
// apps/api/prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// Voir section 6 pour schéma complet
```

```bash
# Commandes Prisma
pnpm prisma generate          # Générer client
pnpm prisma migrate dev       # Créer migration
pnpm prisma migrate deploy    # Appliquer migrations
pnpm prisma studio            # Interface graphique
pnpm prisma db seed          # Peupler base
```

---

## 6. Schéma Prisma Complet

Fichier séparé pour la lisibilité. Voir `21-DATABASE-SCHEMA.md` pour le schéma complet.

---

## 7. Infrastructure & Déploiement

### 7.1 Variables d'Environnement

```bash
# .env.example

# Database
DATABASE_URL="postgresql://user:password@localhost:5432/erp_iut"
REDIS_URL="redis://localhost:6379"

# Auth
JWT_SECRET="your-jwt-secret-here"
JWT_EXPIRES_IN="15m"
REFRESH_TOKEN_SECRET="your-refresh-secret-here"
REFRESH_TOKEN_EXPIRES_IN="7d"

# Frontend
NEXT_PUBLIC_API_URL="http://localhost:3001/api"

# Mobile Money
MOMO_ENV="sandbox"
MOMO_SUBSCRIPTION_KEY="your-key"
MOMO_USER_ID="your-user-id"
MOMO_API_KEY="your-api-key"

# SMS
SMS_API_KEY="your-sms-key"

# File Storage
MINIO_ENDPOINT="localhost"
MINIO_PORT="9000"
MINIO_ROOT_USER="admin"
MINIO_ROOT_PASSWORD="password"
MINIO_BUCKET="erp-files"
```

### 7.2 Déploiement

Voir `23-GETTING-STARTED.md` pour guide complet de déploiement.

---

**Prochaine étape:** Voir [Schéma Base de Données](./21-DATABASE-SCHEMA.md) ou [Guide Démarrage](./23-GETTING-STARTED.md)
