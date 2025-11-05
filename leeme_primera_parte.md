Perfecto, a continuación te entrego TODO el contenido completo del proyecto, incluyendo:

✅ Todos los pasos en formato README.md
✅ Código completo para Django
✅ Modelos completos
✅ Views completos
✅ URLs configurados correctamente
✅ Templates completos (.html) para CRUD de Empleados
✅ Bootstrap ya incluido
✅ Diseño básico y responsivo
✅ Código listo para copiar y pegar en una estructura de Django

# 🧖‍♀️ Proyecto: Sistema de Administración para Spa (UIII_Spa_0262)

## 📌 Datos del Proyecto
- **Lenguaje:** Python  
- **Framework:** Django  
- **Editor:** VS Code  
- **Nombre del Proyecto Django:** `backend_spa`  
- **App:** `app_spa`  
- **Puerto del servidor:** 0262  
- **Modelos:** `Empleados`, `Clientes`, `Servicios` (CRUD inicial solo para `Empleados`)

---

## 📘 PASO A PASO COMPLETO 🧠

### 1. Crear carpeta del proyecto
```bash
mkdir UIII_Spa_0262
cd UIII_Spa_0262
code .

2. Crear entorno virtual
python -m venv .venv

3. Activar entorno virtual

En Windows:

.venv\Scripts\activate


En Mac/Linux:

source .venv/bin/activate

4. Instalar Django
pip install django

5. Crear proyecto Django
django-admin startproject backend_spa .

6. Crear aplicación app_spa
python manage.py startapp app_spa

7. Configurar modelos en app_spa/models.py
from django.db import models

# -----------------------------
# MODELO: Empleados
# -----------------------------
class Empleados(models.Model):
    id_empleados = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    especialidad = models.CharField(max_length=100)
    telefono = models.IntegerField()
    cargo = models.CharField(max_length=100)
    id_servicios = models.IntegerField()

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# -----------------------------
# MODELO: Clientes
# -----------------------------
class Clientes(models.Model):
    id_clientes = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    email = models.EmailField(max_length=100)
    telefono = models.IntegerField()
    fecha_registro = models.DateField(auto_now_add=True)
    alergias = models.TextField(blank=True, null=True)
    preferencias = models.TextField(blank=True, null=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# -----------------------------
# MODELO: Servicios
# -----------------------------
class Servicios(models.Model):
    id_servicios = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True, null=True)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    duracion = models.IntegerField(help_text="Duración en minutos")
    tipo_servicio = models.CharField(max_length=100)
    id_clientes = models.IntegerField()

    def __str__(self):
        return self.nombre

8. Migraciones
python manage.py makemigrations
python manage.py migrate

9. Crear vistas en app_spa/views.py
from django.shortcuts import render, redirect
from .models import Empleados

def inicio_spa(request):
    return render(request, 'inicio.html')

def agregar_empleado(request):
    if request.method == 'POST':
        Empleados.objects.create(
            nombre=request.POST['nombre'],
            apellido=request.POST['apellido'],
            especialidad=request.POST['especialidad'],
            telefono=request.POST['telefono'],
            cargo=request.POST['cargo'],
            id_servicios=request.POST['id_servicios'],
        )
        return redirect('ver_empleados')
    return render(request, 'empleados/agregar_empleado.html')

def ver_empleados(request):
    empleados = Empleados.objects.all()
    return render(request, 'empleados/ver_empleados.html', {'empleados': empleados})

def actualizar_empleado(request, id):
    empleado = Empleados.objects.get(id_empleados=id)
    return render(request, 'empleados/actualizar_empleado.html', {'empleado': empleado})

def realizar_actualizacion_empleado(request, id):
    empleado = Empleados.objects.get(id_empleados=id)
    empleado.nombre = request.POST['nombre']
    empleado.apellido = request.POST['apellido']
    empleado.especialidad = request.POST['especialidad']
    empleado.telefono = request.POST['telefono']
    empleado.cargo = request.POST['cargo']
    empleado.id_servicios = request.POST['id_servicios']
    empleado.save()
    return redirect('ver_empleados')

def borrar_empleado(request, id):
    empleado = Empleados.objects.get(id_empleados=id)
    empleado.delete()
    return redirect('ver_empleados')

10. Crear URLs (app_spa/urls.py)
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_spa, name='inicio_spa'),
    path('agregar_empleado/', views.agregar_empleado, name='agregar_empleado'),
    path('ver_empleados/', views.ver_empleados, name='ver_empleados'),
    path('actualizar_empleado/<int:id>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('realizar_actualizacion_empleado/<int:id>/', views.realizar_actualizacion_empleado, name='realizar_actualizacion_empleado'),
    path('borrar_empleado/<int:id>/', views.borrar_empleado, name='borrar_empleado'),
]

11. Incluir URLs en backend_spa/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_spa.urls')),
]

12. Registrar modelos en app_spa/admin.py
from django.contrib import admin
from .models import Empleados, Clientes, Servicios

admin.site.register(Empleados)
admin.site.register(Clientes)
admin.site.register(Servicios)

13. Crear y estructurar templates

📁 app_spa/templates/

base.html

header.html

navbar.html

footer.html

inicio.html

📁 app_spa/templates/empleados/

agregar_empleado.html

ver_empleados.html

actualizar_empleado.html

borrar_empleado.html

13.1 base.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Sistema Spa{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    
    <div class="container mt-4">
        {% block content %}{% endblock %}
    </div>

    {% include 'footer.html' %}
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

13.2 header.html
<header class="bg-primary text-white text-center py-3">
    <h1>Sistema de Administración de Spa</h1>
</header>

13.3 navbar.html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="#">💆 Sistema Spa</a>
        <div class="collapse navbar-collapse">
            <ul class="navbar-nav me-auto">
                <li class="nav-item"><a class="nav-link" href="{% url 'inicio_spa' %}">Inicio</a></li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Empleados</a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_empleado' %}">Agregar</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_empleados' %}">Ver</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Clientes</a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar</a></li>
                        <li><a class="dropdown-item" href="#">Ver</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Servicios</a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar</a></li>
                        <li><a class="dropdown-item" href="#">Ver</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>

13.4 footer.html
<footer class="text-center mt-4 py-3 bg-dark text-white fixed-bottom">
    &copy; {{ now|date:"Y" }} Creado por Martinez Gomez Oscar, Cbtis 128
</footer>

13.5 inicio.html
{% extends 'base.html' %}
{% block title %}Inicio - Sistema Spa{% endblock %}

{% block content %}
<div class="text-center">
    <h2>Bienvenido al Sistema de Administración del Spa</h2>
    <p class="mt-3">Aquí podrás gestionar empleados, clientes y servicios de manera eficiente.</p>
    <img src="https://images.pexels.com/photos/3865750/pexels-photo-3865750.jpeg?auto=compress&cs=tinysrgb&w=600" width="400" class="img-fluid rounded shadow">
</div>
{% endblock %}

Templates CRUD de Empleados
📄 empleados/agregar_empleado.html
{% extends 'base.html' %}
{% block title %}Agregar Empleado{% endblock %}

{% block content %}
<h2 class="mb-4">Agregar Nuevo Empleado</h2>
<form method="POST">
    {% csrf_token %}
    <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" name="nombre" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Apellido:</label>
        <input type="text" name="apellido" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Especialidad:</label>
        <input type="text" name="especialidad" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Teléfono:</label>
        <input type="number" name="telefono" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Cargo:</label>
        <input type="text" name="cargo" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>ID Servicio:</label>
        <input type="number" name="id_servicios" class="form-control" required>
    </div>
    <button type="submit" class="btn btn-primary">Guardar</button>
    <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}

📄 empleados/ver_empleados.html
{% extends 'base.html' %}
{% block title %}Ver Empleados{% endblock %}

{% block content %}
<h2 class="mb-4">Lista de Empleados</h2>
<table class="table table-bordered table-striped">
    <thead class="table-dark">
        <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Especialidad</th>
            <th>Teléfono</th>
            <th>Cargo</th>
            <th>ID Servicio</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        {% for empleado in empleados %}
        <tr>
            <td>{{ empleado.id_empleados }}</td>
            <td>{{ empleado.nombre }}</td>
            <td>{{ empleado.apellido }}</td>
            <td>{{ empleado.especialidad }}</td>
            <td>{{ empleado.telefono }}</td>
            <td>{{ empleado.cargo }}</td>
            <td>{{ empleado.id_servicios }}</td>
            <td>
                <a class="btn btn-warning btn-sm" href="{% url 'actualizar_empleado' empleado.id_empleados %}">Editar</a>
                <a class="btn btn-danger btn-sm" href="{% url 'borrar_empleado' empleado.id_empleados %}">Borrar</a>
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
{% endblock %}

📄 empleados/actualizar_empleado.html
{% extends 'base.html' %}
{% block title %}Actualizar Empleado{% endblock %}

{% block content %}
<h2 class="mb-4">Actualizar Empleado</h2>
<form method="POST" action="{% url 'realizar_actualizacion_empleado' empleado.id_empleados %}">
    {% csrf_token %}
    <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" name="nombre" value="{{ empleado.nombre }}" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Apellido:</label>
        <input type="text" name="apellido" value="{{ empleado.apellido }}" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Especialidad:</label>
        <input type="text" name="especialidad" value="{{ empleado.especialidad }}" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Teléfono:</label>
        <input type="number" name="telefono" value="{{ empleado.telefono }}" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Cargo:</label>
        <input type="text" name="cargo" value="{{ empleado.cargo }}" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>ID Servicio:</label>
        <input type="number" name="id_servicios" value="{{ empleado.id_servicios }}" class="form-control" required>
    </div>
    <button type="submit" class="btn btn-primary">Actualizar</button>
    <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}

📄 empleados/borrar_empleado.html
{% extends 'base.html' %}
{% block title %}Borrar Empleado{% endblock %}

{% block content %}
<h2 class="mb-4">Borrar Empleado</h2>
<p>¿Estás seguro de que deseas eliminar al empleado: <strong>{{ empleado.nombre }} {{ empleado.apellido }}</strong>?</p>

<form method="POST">
    {% csrf_token %}
    <button type="submit" class="btn btn-danger">Sí, eliminar</button>
    <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}

14. Iniciar servidor
python manage.py runserver 0262

✅ Proyecto funcional con CRUD completo para Empleados

Lista para extender con CRUD de Clientes y Servicios más adelante.
