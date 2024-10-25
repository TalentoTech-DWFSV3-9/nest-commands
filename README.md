# NestJS Command Cheat Sheet

Este repositorio contiene una recopilación de los comandos más comunes y útiles para trabajar con NestJS.

## Instalación y Configuración Básica

```bash
# Instalar la CLI de NestJS
npm i -g @nestjs/cli
```
```bash
# Crear un nuevo proyecto en el directorio actual
nest new project-name
```

```bash
# Ver ayuda para cualquier comando
nest -h
```

## Generación de Componentes

```bash
# Generar cualquier componente (general)
nest generate <componente>
nest g <componente>

# Generar componentes específicos
nest g cl <path/nombre>          # Clase
nest g co <path/nombre>          # Controlador
nest g d <path/nombre>           # Decorador
nest g gu <path/nombre>          # Guard
nest g in <path/nombre>          # Interceptor
nest g mo <path/nombre>          # Módulo
nest g pi <path/nombre>          # Pipe
nest g s <path/nombre>           # Servicio
nest g resource <nombre>         # Recurso completo

# Confirmar cambios sin ejecutarlos
nest g s nombre --dry-run | -d

# Generar sin archivo de pruebas
nest g s nombre --no-spec
```

## Decoradores y Métodos HTTP

```typescript
import { Get, Post, Put, Delete, Param, Body, Query, Res } from '@nestjs/common';

// Métodos HTTP básicos
@Get()
@Post()
@Put()
@Delete()

// Rutas y parámetros dinámicos
@Get(':id')                      // Ruta con parámetro dinámico
@Get('ruta/especifica')          // Ruta específica

// Obtener parámetros y cuerpo de la solicitud
@Param('id')                     // Obtener parámetro de URL
@Body()                          // Obtener body de la petición
@Query()                         // Obtener parámetros de query
@Res()                           // Obtener respuesta de Express/Fastify
```

## Pipes de Validación y Transformación

Algunos pipes útiles integrados en NestJS:

```typescript
import { ParseIntPipe, ValidationPipe } from '@nestjs/common';

@Param('id', ParseIntPipe) id: number       // Convertir a entero

// Configuración global de pipes
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,                     // Remueve propiedades no especificadas
    forbidNonWhitelisted: true           // Retorna error si existen propiedades no requeridas
  })
);
```

## Manejo de Errores y Excepciones

NestJS cuenta con varias excepciones integradas:

- `BadRequestException`
- `UnauthorizedException`
- `NotFoundException`
- `ForbiddenException`
- `RequestTimeoutException`
- `InternalServerErrorException`

Ejemplo de manejo de excepciones personalizado:

```typescript
throw new NotFoundException('Recurso no encontrado');
```

## Configuración Adicional

```typescript
// Habilitar CORS
app.enableCors();                   // Habilitar con opciones por defecto
app.enableCors(options);            // Habilitar con opciones personalizadas

// Uso de archivos de configuración .env
npm i @nestjs/config

@Module({
  imports: [ConfigModule.forRoot()]
})
export class AppModule {}

// Servir contenido estático
npm i @nestjs/serve-static

import { ServeStaticModule } from '@nestjs/serve-static';
import { join } from 'path';

@Module({
  imports: [
    ServeStaticModule.forRoot({
      rootPath: join(__dirname, '..', 'public'),
    }),
  ],
})
export class AppModule {}
```

## Librerías y Decoradores Externos Útiles

Instalación de decoradores de validación con `class-validator` y `class-transformer`:

```bash
npm i class-validator class-transformer
```

Decoradores comunes:

```typescript
import { IsString, IsUUID, IsOptional, IsEmail } from 'class-validator';

@IsString()
@IsUUID()
@IsOptional()
@IsEmail()
```

