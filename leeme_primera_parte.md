Te ayudo a crear el proyecto SPA completo paso a paso. Aquí está la estructura completa

🔧 Paso 1: Crear Carpeta del Proyecto
# 1. Crear carpeta del proyecto
    mkdir UIII_Spa_0262
    cd UIII_Spa_0262

🔧 Paso 2: Abrir VS Code
# 2. Abrir VS Code sobre la carpeta
    code .

🔧 Paso 3: Abrir Terminal en VS Code
    Presiona Ctrl + Ñ (Windows/Linux) o Cmd + Ñ (Mac)
    O ve al menú: View > Terminal

🔧 Paso 4: Crear Entorno Virtual
# 4. Crear entorno virtual
    python -m venv .venv

🔧 Paso 5: Activar Entorno Virtual

# 5. Activar entorno virtual (Windows)
    .\.venv\Scripts\activate

# Para Mac/Linux:
#    source .venv/bin/activate

🔧 Paso 6: Activar Intérprete de Python
Presiona Ctrl + Shift + P
Escribe: "Python: Select Interpreter"
Selecciona el interprete de .venv

🔧 Paso 7: Instalar Django
# 7. Instalar Django
    pip install django
    pip freeze > requirements.txt

🔧 Paso 8: Crear Proyecto Backend
# 8. Crear proyecto sin duplicar carpeta
    django-admin startproject backend_spa .

🔧 Paso 9: Ejecutar Servidor en Puerto 0262
# 9. Ejecutar servidor
    python manage.py runserver 0262

🔧 Paso 10: Probar en Navegador
    Copia y pega: http://127.0.0.1:0262/ en tu navegador

🔧 Paso 11: Crear Aplicación
# 11. Crear aplicación
    python manage.py startapp app_spa

🗃️ Paso 12: Modelos
✅ app_spa/models.py

    python
    from django.db import models

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

🔧 Paso 12.5: Realizar Migraciones

    bash
    python manage.py makemigrations
    python manage.py migrate

🧠 Paso 13-14: Vistas para Empleados
✅ app_spa/views.py

    python
    from django.shortcuts import render, redirect, get_object_or_404
    from .models import Empleados

    def inicio_spa(request):
    return render(request, 'inicio.html')

    def agregar_empleado(request):
    if request.method == 'POST':
        empleado = Empleados(
            nombre=request.POST['nombre'],
            apellido=request.POST['apellido'],
            especialidad=request.POST['especialidad'],
            telefono=request.POST['telefono'],
            cargo=request.POST['cargo'],
            id_servicios=request.POST['id_servicios']
        )
        empleado.save()
        return redirect('ver_empleados')
    return render(request, 'empleados/agregar_empleado.html')

    def ver_empleados(request):
    empleados = Empleados.objects.all()
    return render(request, 'empleados/ver_empleados.html', {'empleados': empleados})

    def actualizar_empleado(request, id):
    empleado = get_object_or_404(Empleados, id_empleados=id)
    return render(request, 'empleados/actualizar_empleado.html', {'empleado': empleado})

    def realizar_actualizacion_empleado(request, id):
    if request.method == 'POST':
        empleado = get_object_or_404(Empleados, id_empleados=id)
        empleado.nombre = request.POST['nombre']
        empleado.apellido = request.POST['apellido']
        empleado.especialidad = request.POST['especialidad']
        empleado.telefono = request.POST['telefono']
        empleado.cargo = request.POST['cargo']
        empleado.id_servicios = request.POST['id_servicios']
        empleado.save()
        return redirect('ver_empleados')
    return redirect('ver_empleados')

    def borrar_empleado(request, id):
    empleado = get_object_or_404(Empleados, id_empleados=id)
    if request.method == 'POST':
        empleado.delete()
        return redirect('ver_empleados')
    return render(request, 'empleados/borrar_empleado.html', {'empleado': empleado})

🎨 Paso 15-22: Plantillas HTML
✅ app_spa/templates/base.html

    html
    <!DOCTYPE html>
    <html lang="es">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema SPA</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8f9fa;
            padding-top: 20px;
        }
        .main-content {
            min-height: calc(100vh - 120px);
        }
        .navbar-custom {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .footer-custom {
            background-color: #343a40;
            color: white;
            margin-top: auto;
        }
    </style>
    </head>
    <body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    
    <div class="container main-content">
        {% block content %}
        {% endblock %}
    </div>
    
    {% include 'footer.html' %}
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    </body>
    </html>
✅ app_spa/templates/header.html
    
    html
    <header class="text-center mb-4">
    <h1 class="display-4 text-primary">💆 Sistema de Administración SPA</h1>
    <p class="lead text-muted">Gestión integral de empleados, clientes y servicios</p>
    </header>

✅ app_spa/templates/navbar.html

    html
    <nav class="navbar navbar-expand-lg navbar-dark navbar-custom mb-4">
    <div class="container">
        <a class="navbar-brand" href="{% url 'inicio_spa' %}">
            🏠 Sistema SPA
        </a>
        
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
            <span class="navbar-toggler-icon"></span>
        </button>
        
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav me-auto">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'inicio_spa' %}">
                        📊 Inicio
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        👥 Empleados
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_empleado' %}">Agregar Empleado</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_empleados' %}">Ver Empleados</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#">Actualizar Empleado</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Empleado</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        👨‍💼 Clientes
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Cliente</a></li>
                        <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#">Actualizar Cliente</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Cliente</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        💅 Servicios
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Servicio</a></li>
                        <li><a class="dropdown-item" href="#">Ver Servicios</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#">Actualizar Servicio</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Servicio</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
    </nav>
✅ app_spa/templates/footer.html

    html
    <footer class="footer-custom py-3 mt-5">
    <div class="container text-center">
        <p class="mb-0">
            &copy; 2025 Sistema SPA - Todos los derechos reservados | 
            Fecha: {% now "d/m/Y" %} | 
            Creado por Martinez Gomez Oscar, Cbtis 128
        </p>
    </div>
    </footer>
✅ app_spa/templates/inicio.html

    html
    {% extends 'base.html' %}

    {% block content %}
    <div class="row">
    <div class="col-md-8">
        <div class="card">
            <div class="card-body">
                <h2 class="card-title text-primary">Bienvenido al Sistema SPA</h2>
                <p class="card-text">
                    Este sistema permite la gestión integral de tu spa, incluyendo:
                </p>
                <ul>
                    <li>Gestión de empleados y sus especialidades</li>
                    <li>Registro y seguimiento de clientes</li>
                    <li>Control de servicios y tratamientos</li>
                    <li>Gestión de citas y horarios</li>
                </ul>
                <p class="text-muted">
                    Utiliza el menú de navegación para acceder a las diferentes secciones del sistema.
                </p>
            </div>
        </div>
    </div>
    <div class="col-md-4">
        <div class="card">
            <div class="card-body text-center">
                <img src="https://images.unsplash.com/photo-1544161515-4ab6ce6db874?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" 
                     alt="SPA Relajante" class="img-fluid rounded">
                <p class="mt-3 text-muted">Ambiente relajante y profesional</p>
            </div>
        </div>
    </div>
    </div>
    {% endblock %}
✅ app_spa/templates/empleados/agregar_empleado.html

    html
    {% extends 'base.html' %}

    {% block content %}
    <div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card">
            <div class="card-header bg-primary text-white">
                <h4 class="mb-0">👥 Agregar Nuevo Empleado</h4>
            </div>
            <div class="card-body">
                <form method="POST">
                    {% csrf_token %}
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Nombre</label>
                            <input type="text" class="form-control" name="nombre" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Apellido</label>
                            <input type="text" class="form-control" name="apellido" required>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Especialidad</label>
                            <input type="text" class="form-control" name="especialidad" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Teléfono</label>
                            <input type="number" class="form-control" name="telefono" required>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Cargo</label>
                            <input type="text" class="form-control" name="cargo" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">ID Servicios</label>
                            <input type="number" class="form-control" name="id_servicios" required>
                        </div>
                    </div>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <button type="submit" class="btn btn-primary">Guardar Empleado</button>
                        <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
    </div>
    {% endblock %}
✅ app_spa/templates/empleados/ver_empleados.html

    html
    {% extends 'base.html' %}

    {% block content %}
    <div class="card">
    <div class="card-header bg-success text-white d-flex justify-content-between align-items-center">
        <h4 class="mb-0">📋 Lista de Empleados</h4>
        <a href="{% url 'agregar_empleado' %}" class="btn btn-light">➕ Agregar Empleado</a>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead class="table-dark">
                    <tr>
                        <th>ID</th>
                        <th>Nombre</th>
                        <th>Apellido</th>
                        <th>Especialidad</th>
                        <th>Teléfono</th>
                        <th>Cargo</th>
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
                        <td>
                            <a href="{% url 'actualizar_empleado' empleado.id_empleados %}" class="btn btn-warning btn-sm">✏️ Editar</a>
                            <a href="{% url 'borrar_empleado' empleado.id_empleados %}" class="btn btn-danger btn-sm">🗑️ Borrar</a>
                        </td>
                    </tr>
                    {% empty %}
                    <tr>
                        <td colspan="7" class="text-center text-muted">No hay empleados registrados</td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
    </div>
    </div>
    {% endblock %}
✅ app_spa/templates/empleados/actualizar_empleado.html

    html
    {% extends 'base.html' %}

    {% block content %}
    <div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card">
            <div class="card-header bg-warning text-dark">
                <h4 class="mb-0">✏️ Actualizar Empleado</h4>
            </div>
            <div class="card-body">
                <form method="POST" action="{% url 'realizar_actualizacion_empleado' empleado.id_empleados %}">
                    {% csrf_token %}
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Nombre</label>
                            <input type="text" class="form-control" name="nombre" value="{{ empleado.nombre }}" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Apellido</label>
                            <input type="text" class="form-control" name="apellido" value="{{ empleado.apellido }}" required>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Especialidad</label>
                            <input type="text" class="form-control" name="especialidad" value="{{ empleado.especialidad }}" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Teléfono</label>
                            <input type="number" class="form-control" name="telefono" value="{{ empleado.telefono }}" required>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Cargo</label>
                            <input type="text" class="form-control" name="cargo" value="{{ empleado.cargo }}" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">ID Servicios</label>
                            <input type="number" class="form-control" name="id_servicios" value="{{ empleado.id_servicios }}" required>
                        </div>
                    </div>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <button type="submit" class="btn btn-warning">Actualizar Empleado</button>
                        <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
    </div>
    {% endblock %}
✅ app_spa/templates/empleados/borrar_empleado.html

    html
    {% extends 'base.html' %}

    {% block content %}
    <div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card">
            <div class="card-header bg-danger text-white">
                <h4 class="mb-0">🗑️ Confirmar Eliminación</h4>
            </div>
            <div class="card-body text-center">
                <h5>¿Estás seguro de que quieres eliminar a este empleado?</h5>
                <p class="text-muted">{{ empleado.nombre }} {{ empleado.apellido }}</p>
                <p><strong>Especialidad:</strong> {{ empleado.especialidad }}</p>
                <p><strong>Cargo:</strong> {{ empleado.cargo }}</p>
                
                <form method="POST">
                    {% csrf_token %}
                    <div class="d-grid gap-2 d-md-flex justify-content-md-center">
                        <button type="submit" class="btn btn-danger">Sí, Eliminar</button>
                        <a href="{% url 'ver_empleados' %}" class="btn btn-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
    </div>
    {% endblock %}

🔗 Paso 23-24: URLs de la Aplicación
✅ app_spa/urls.py

    python
    from django.urls import path
    from . import views

    urlpatterns = [
    path('', views.inicio_spa, name='inicio_spa'),
    path('agregar-empleado/', views.agregar_empleado, name='agregar_empleado'),
    path('ver-empleados/', views.ver_empleados, name='ver_empleados'),
    path('actualizar-empleado/<int:id>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('realizar-actualizacion-empleado/<int:id>/', views.realizar_actualizacion_empleado, name='realizar_actualizacion_empleado'),
    path('borrar-empleado/<int:id>/', views.borrar_empleado, name='borrar_empleado'),
    ]

⚙️ Paso 25: Configuración Settings
✅ backend_spa/settings.py

    python
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_spa',  # Agregar esta línea
    ]

# Agregar al final del archivo

    import os
    STATIC_URL = '/static/'
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

🔗 Paso 26: URLs del Proyecto
✅ backend_spa/urls.py

    python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_spa.urls')),
    ]

👨‍💼 Paso 27: Registrar Modelos en Admin
✅ app_spa/admin.py

    python
    from django.contrib import admin
    from .models import Empleados, Clientes, Servicios

    @admin.register(Empleados)
    class EmpleadosAdmin(admin.ModelAdmin):
    list_display = ('id_empleados', 'nombre', 'apellido', 'especialidad', 'cargo')
    search_fields = ('nombre', 'apellido', 'especialidad')

    @admin.register(Clientes)
    class ClientesAdmin(admin.ModelAdmin):
    list_display = ('id_clientes', 'nombre', 'apellido', 'email', 'fecha_registro')
    search_fields = ('nombre', 'apellido', 'email')

    @admin.register(Servicios)
    class ServiciosAdmin(admin.ModelAdmin):
    list_display = ('id_servicios', 'nombre', 'precio', 'duracion', 'tipo_servicio')
    search_fields = ('nombre', 'tipo_servicio')

🚀 Paso Final: Ejecutar Proyecto

    bash
# Realizar migraciones finales
    python manage.py makemigrations
    python manage.py migrate

# Crear superusuario (opcional)

    python manage.py createsuperuser

# Ejecutar servidor en puerto 0262

    python manage.py runserver 0262

✅ Funcionalidades Completas
✅ Sistema completo de gestión SPA
✅ CRUD completo para empleados
✅ Diseño moderno con Bootstrap
✅ Estructura profesional
✅ Navegación responsive
✅ Plantillas organizadas

🌐 Acceso al Sistema

    Sitio web: http://127.0.0.1:0262/
    Panel admin: http://127.0.0.1:0262/admin/
