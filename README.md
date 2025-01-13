# Stack de Aplicación con Docker Compose

## Componentes del Stack
- Nginx (Frontend)
- PostgreSQL 15 (Base de datos)
- Redis (Cache)

## Requisitos Previos
- Docker
- Docker Compose
- Mínimo 2GB RAM disponible
- Puertos disponibles: 8000, 5432, 6379

## Inicio Rápido

```bash
# Iniciar servicios
docker-compose up -d

# Verificar estado
docker-compose ps

# Ver logs
docker-compose logs -f
```

## Configuración de Servicios

### Nginx (Frontend)
- Puerto: 8000
- Base: Alpine Linux
- Reinicio automático configurado
- Límite de logs: 10MB x 3 archivos

### PostgreSQL
- Versión: 15 Alpine
- Puerto: 5432
- Credenciales:
  - Usuario: admin
  - Contraseña: password123
  - Base de datos: my_database
- Volumen persistente: pg_data
- Healthcheck cada 10s

### Redis
- Versión: Alpine
- Puerto: 6379
- Contraseña: redispass123
- Healthcheck cada 10s

## Comandos Útiles

### Gestión de Servicios
```bash
# Detener servicios
docker-compose down

# Reiniciar un servicio específico
docker-compose restart [servicio]

# Ver logs de un servicio
docker-compose logs [servicio]
```

### Base de Datos
```bash
# Conectar a PostgreSQL
docker-compose exec database psql -U admin -d my_database

# Backup de base de datos
docker-compose exec database pg_dump -U admin my_database > backup.sql
```

### Redis
```bash
# Conectar a Redis
docker-compose exec redis redis-cli -a redispass123

# Verificar estado
docker-compose exec redis redis-cli -a redispass123 ping
```

## Solución de Problemas

### Verificar Estado
```bash
# Estado de contenedores
docker-compose ps

# Uso de recursos
docker stats
```

### Problemas Comunes

1. **Error de Conexión a PostgreSQL**:
   ```bash
   docker-compose logs database
   ```

2. **Error de Memoria en Redis**:
   ```bash
   docker-compose restart redis
   ```

3. **Nginx No Responde**:
   ```bash
   docker-compose logs app
   ```

## Mantenimiento

### Backups
```bash
# Backup de volumen PostgreSQL
docker run --rm --volumes-from compose_database_1 -v $(pwd):/backup alpine tar cvf /backup/pg_backup.tar /var/lib/postgresql/data
```

### Actualizaciones
```bash
# Actualizar imágenes
docker-compose pull

# Reiniciar servicios
docker-compose up -d
```

## Seguridad

Recomendaciones:
1. Cambiar contraseñas por defecto
2. Limitar acceso a puertos
3. Configurar red interna
4. Implementar SSL/TLS